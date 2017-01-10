<?php

/*
*
* Decode html into a multidimensional array
*
* @html	html code to be decoded
* @tags	the tags you'd like to include, defaults are div|form
* @searchable_by	the attributes you'd like to search by, defaults are id|class
* @return      array
*
*/	
function html_decode($html,$tags = 'div|form', $searchable_by = 'id|class') {
    $html = preg_replace('/>\s+</', '><',preg_replace('/\s+/', ' ',$html));

    $regex_element = '/<('.$tags.')([^>]*)>((?!<\/?('.$tags.')[^>]*>).)*<\/\1[^>]?>/i';
    $regex_search = '/('.$searchable_by.')\s?=\s?[\'|"]([^\'|"]*)[\'|"]/i';
    $token_template = '{{%s:html_token}}';

    $data = array(); // holds the decoded data
    $child_elements = array(); // used to track what elements are children
    $result = array('data'=> array(),'search' => array()); // the value that will be returned

    while(preg_match_all($regex_element,$html,$matches)) {
        foreach ($matches[0] AS $key => $element) {
            $token = sprintf($token_template,sizeof($data));
            $html = str_replace($element,$token,$html);

            $data[$token] = array(
                'tag' => $matches[1][$key],
                'attributes' => array(),
                'children' => array(),
            );

            // find tokens and replace them with their html and set them as children
            if (preg_match_all('/{{\d+:html_token}}/', $element, $child_tokens)) {
                foreach ($child_tokens[0] AS $child_token) {
                    $child_elements[$child_token] = true;
                    $data[$token]['children'][$data[$child_token]['tag'].sizeof($data[$token]['children'])] = $data[$child_token];
                    $element = str_replace($child_token,$data[$child_token]['html'],$element);
                }
            }

            // find the attributes and make them searchable
            if (preg_match_all($regex_search, $matches[2][$key], $attributes)) {
                foreach ($attributes[0] AS $index => $attribute) {
                    $data[$token]['attributes'][$attributes[1][$index]] = $attributes[2][$index];
                    $search_values = explode(' ', $attributes[2][$index]);
                    foreach ($search_values AS $search_value) {
                        $result['search'][$attributes[1][$index].'='.$search_value][] =& $data[$token];
                    }
                }
            }

            $data[$token]['html'] = $element;
        }
    }

    $data = array_reverse(array_diff_key($data,$child_elements));
    foreach ($data AS $key => $value) {
        $result['data'][$value['tag'].sizeof($result['data'])] = $value;
    }

    return $result;
}

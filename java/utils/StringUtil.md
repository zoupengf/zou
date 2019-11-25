package com.keytop.commons.utils;


import org.apache.commons.lang3.StringUtils;

import java.util.*;

public class StringUtil extends StringUtils {

    //首字母转小写
    public static String toLowerCaseFirstOne(String s) {
        if (Character.isLowerCase(s.charAt(0)))
            return s;
        else
            return (new StringBuilder()).append(Character.toLowerCase(s.charAt(0))).append(s.substring(1)).toString();
    }


    //首字母转大写
    public static String toUpperCaseFirstOne(String s) {
        if (Character.isUpperCase(s.charAt(0)))
            return s;
        else
            return (new StringBuilder()).append(Character.toUpperCase(s.charAt(0))).append(s.substring(1)).toString();
    }

    public static List<String> strToList(String strs) {
        if (isNotBlank(strs)) {
            return Arrays.asList(split(strs, ";"));
        }
        return new ArrayList<>();
    }

    public static List<Long> longToList(String strs) {
        List<Long> list = new ArrayList<>();
        if (isNotBlank(strs)) {
            String[] ss = split(strs, ";");
            for (String s : ss) {
                list.add(Long.parseLong(s));
            }
        }
        return list;
    }

    public static Set<String> strToSet(String strs) {
        if (isNotBlank(strs)) {
            return new HashSet(Arrays.asList(split(strs, ";")));
        }
        return new HashSet<>();
    }

    public static Set<Long> longToSet(String strs) {
        Set<Long> sets = new HashSet<>();
        if (isNotBlank(strs)) {
            String[] ss = split(strs, ";");
            for (String s : ss) {
                sets.add(Long.parseLong(s));
            }
        }
        return sets;
    }

}

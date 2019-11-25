package com.keytop.commons.utils;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import org.springframework.util.LinkedMultiValueMap;
import org.springframework.util.MultiValueMap;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.*;

public class ObjectUtils {

    /**
     * 对象转实map
     *
     * @param obj
     * @return
     */
    public static Map<String, Object> objectToMap(Object obj){
        JSONObject jsonObject = (JSONObject) JSON.toJSON(obj);
        Set<Map.Entry<String, Object>> entrySet = jsonObject.entrySet();
        Map<String, Object> map = new HashMap();
        for (Map.Entry<String, Object> entry : entrySet) {
            map.put(entry.getKey(), entry.getValue());
        }
        return map;
    }

    /**
     * @auther: caoqiuliang
     *
     * @Title: objectToMultiValueMap
     * @Description: 获取MultiValueMap对象
     * @Date: 2019/7/15 15:20
     * @param: obj
     * @return: org.springframework.util.MultiValueMap
     * @throws:
     */
    public static MultiValueMap objectToMultiValueMap(Object obj){
        JSONObject jsonObject = (JSONObject) JSON.toJSON(obj);
        Set<Map.Entry<String, Object>> entrySet = jsonObject.entrySet();


        MultiValueMap multiValueMap = new LinkedMultiValueMap();
        for (Map.Entry<String, Object> entry : entrySet) {
            multiValueMap.add(entry.getKey(), entry.getValue());
        }
        return multiValueMap;
    }

    public static <T> T parseMap2Object(Map<String, Object> paramMap, Class<T> cls) {
        return JSONObject.parseObject(JSONObject.toJSONString(paramMap), cls);
    }

    /**
     * @auther: qiuqinghua
     * @Title: sort
     * @Description: 实体排序
     * @Date: 2018/11/16 13:37
     * @param:
     * @return:
     * @throws:
     */
    public static void sort(List<Object> targetList, final String sortField, final String sortMode) {
        Collections.sort(targetList, new Comparator() {
            public int compare(Object obj1, Object obj2) {
                int retVal = 0;
                try {
                    //首字母转大写
                    String newStr = sortField.substring(0, 1).toUpperCase() + sortField.replaceFirst("\\w", "");
                    String methodStr = "get" + newStr;
                    Method method1 = obj1.getClass().getMethod(methodStr);
                    Method method2 = obj2.getClass().getMethod(methodStr);
                    if (sortMode != null && "desc".equals(sortMode)) {
                        retVal = method2.invoke((obj2)).toString().compareTo(method1.invoke((obj1)).toString()); // 倒序
                    } else {
                        retVal = method1.invoke((obj1)).toString().compareTo(method2.invoke((obj2)).toString()); // 正序
                    }
                } catch (Exception e) {
                    throw new RuntimeException();
                }
                return retVal;
            }
        });
    }
    /**
     * @auther: qiuqinghua
     * 
     * @Title: removeDuplicateCase
     * @Description:
     * @Date: 2018/11/16 13:52
     * @param: 去重
     * @return: 
     * @throws: 
     */
    public static List<Object> removeDuplicateCase(List<Object> ls, String filed) {
        Set<Object> set = new TreeSet<>((o1, o2) -> {
            String newStr = filed.substring(0, 1).toUpperCase() + filed.replaceFirst("\\w", "");
            String methodStr = "get" + newStr;
            if(o1==null ||o2==null)
                return 0;
            try {
                String strO1 = "";
                String strO2 = "";
                if(o1 instanceof Map){
                    strO1 = ((Map)o1).get(filed)+"";
                }else{
                    Method method1 = o1.getClass().getMethod(methodStr, null);
                    strO1 = method1.invoke((o1), null).toString();
                }
                if(o2 instanceof Map){
                    strO2 = ((Map)o2).get(filed)+"";
                }else{
                    Method method2 = o2.getClass().getMethod(methodStr, null);
                    strO2 = method2.invoke((o2), null).toString();
                }
                return strO2.compareTo(strO1);
            } catch (NoSuchMethodException e) {
                e.printStackTrace();
            } catch (IllegalAccessException e) {
                e.printStackTrace();
            } catch (InvocationTargetException e) {
                e.printStackTrace();
            }
            return 0;
        });
        set.addAll(ls);
        return new ArrayList<>(set);
    }

    /**
     * @auther: cxx
     *
     * @Title: List<String>去重
     * @Description:
     * @Date: 2019/3/4 13:56
     * @param:
     * @return:
     * @throws:
     */
    public   static   List  removeDuplicate(List<String> list)  {
        for  ( int  i  =   0 ; i  <  list.size()  -   1 ; i ++ )  {
            for  ( int  j  =  list.size()  -   1 ; j  >  i; j -- )  {
                if  (list.get(j).equals(list.get(i)))  {
                    list.remove(j);
                }
            }
        }
        return list;
    }
}

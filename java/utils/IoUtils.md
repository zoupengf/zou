package com.keytop.commons.utils;

import java.io.*;

public class IoUtils {

    public static Object toObject(byte[] bytes) {
        Object obj = null;
        try {
            if (bytes != null && bytes.length > 0) {
                ByteArrayInputStream bis = new ByteArrayInputStream(bytes);
                ObjectInputStream ois = new ObjectInputStream(bis);
                obj = ois.readObject();
                ois.close();
                bis.close();
            }
        } catch (Exception e) {
            return null;
        }
        return obj;
    }

    public static byte[] toByteArray(Object obj) {
        byte[] bytes = null;
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        try {
            ObjectOutputStream oos = new ObjectOutputStream(bos);
            oos.writeObject(obj);
            oos.flush();
            bytes = bos.toByteArray();
            oos.close();
            bos.close();
        } catch (IOException ex) {
            ex.printStackTrace();
        }
        return bytes;
    }
}

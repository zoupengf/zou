/**
 * Copyright (c) 2013-Now http://jeesite.com All rights reserved.
 */
package com.keytop.commons.utils;

import java.math.BigDecimal;
import java.text.DecimalFormat;

/**
 * BigDecimal工具类
 * @author ThinkGem
 * @version 2016年6月11日
 */
public class NumberUtils extends org.apache.commons.lang3.math.NumberUtils {

	/**
	 * 获取BigDecimal，若为空则使用提供的默认值
	 * @param bigDecimal
	 * @param defaultBigDecimal 默认值
	 * @return
	 */
	public static BigDecimal getBigDecimal(BigDecimal bigDecimal, BigDecimal defaultBigDecimal){
		return bigDecimal == null ? defaultBigDecimal : bigDecimal;
	}

	/**
	 * 相加运算
	 * @param v1
	 * @param v2
	 * @return 若v1、v2同时为null返回0，若v1为null则返回v2，若v2为null则返回v1，否则返回计算结算
	 */
	public static BigDecimal addNullable(BigDecimal v1, BigDecimal v2) {
		if(v1 == null && v2 == null){
			return new BigDecimal("0");
		}
		if(v1 == null){
			return v2;
		}
		if(v2 == null){
			return v1;
		}
		return v1.add(v2);
	}

	/**
	 * 相减运算
	 * @param v1
	 * @param v2
	 * @return 若v1、v2同时为null返回0，否则若v1为null返回-v2，若v2为null则返回v1,否则返回计算结算
	 */
	public static BigDecimal subNullable(BigDecimal v1, BigDecimal v2) {
		if(v1 == null && v2 == null){
			return new BigDecimal("0");
		}
		if(v1 == null){
			return v2.negate();
		}

		if(v2 == null){
			return v1;
		}
		return v1.subtract(v2);
	}

	/**
	 * 相乘运算
	 * @param v1
	 * @param v2
	 * @return 若v1 或 v2为null则返回0，否则返回计算结算
	 */
	public static BigDecimal mulNullable(BigDecimal v1, BigDecimal v2) {
		if(v1 == null || v2 == null){
			return BigDecimal.ZERO;
		}

		// 解决计算结果有6个0时，计算结果为0E-7的问题
		v1 = v1.stripTrailingZeros();
		v2 = v2.stripTrailingZeros();

		return v1.multiply(v2);
	}

	/**
	 * 两个数字相除
	 * 	 	若v1 为null，则返回0
	 * 	 	若v2为null，则返回v1
	 * 	  	保留两位小数
	 *
	 * @param v1
	 * @param v2
	 * @return 若v1 或 v2为null则返回0，否则返回计算结算
	 */
	public static BigDecimal divNullable(BigDecimal v1, BigDecimal v2) {
		return divNullable(v1,v2,2);
	}

	/**
     * 两个数字相除
     * 	 	若v1 为null，则返回0
     * 	 	若v2为null，则返回v1
	 * @param v1
	 * @param v2
	 * @param scale 保留多少位小数,负数则不保留小数
	 * @return 若v1 或 v2为null则返回0，否则返回计算结算
	 */
	public static BigDecimal divNullable(BigDecimal v1, BigDecimal v2, int scale) {
		return divNullable(v1, v2, scale, BigDecimal.ROUND_HALF_UP);
	}

	/**
	 * 两个数字相除
	 * 	 若v1 为null，则返回0
	 * 	 若v2为null，则返回v1
	 * @param v1
	 * @param v2
	 * @param scale 保留位数，小于0则不保留小数
	 * @param roundingMode 舍入模式
	 * @return
	 */
	public static BigDecimal divNullable(BigDecimal v1, BigDecimal v2, int scale, int roundingMode) {
		if (scale < 0) {
			throw new IllegalArgumentException("scale must not less than 0");
		}

		if(v1 == null){
			return BigDecimal.ZERO;
		}

		if(v2 == null){
			return v1;
		}

		if (BigDecimal.ZERO.equals(v2)) {
			throw new IllegalArgumentException("v2 must not be zero");
		}

		// 解决计算结果有6个0时，计算结果为0E-7的问题
		v1 = v1.stripTrailingZeros();
		v2 = v2.stripTrailingZeros();

		return v1.divide(v2, scale, roundingMode);
	}

	/**
	 * 提供精确的减法运算。
	 * @param v1 被减数
	 * @param v2 减数
	 * @return 两个参数的差
	 */
	public static double sub(double v1, double v2) {
		BigDecimal b1 = new BigDecimal(Double.toString(v1));
		BigDecimal b2 = new BigDecimal(Double.toString(v2));
		return b1.subtract(b2).doubleValue();
	}

	/**
	 * 提供精确的加法运算。
	 * @param v1 被加数
	 * @param v2 加数
	 * @return 两个参数的和
	 */
	public static double add(double v1, double v2) {
		BigDecimal b1 = new BigDecimal(Double.toString(v1));
		BigDecimal b2 = new BigDecimal(Double.toString(v2));
		return b1.add(b2).doubleValue();
	}

	/**
	 * 提供精确的乘法运算。
	 * @param v1 被乘数
	 * @param v2 乘数
	 * @return 两个参数的积
	 */
	public static double mul(double v1, double v2) {
		BigDecimal b1 = new BigDecimal(Double.toString(v1));
		BigDecimal b2 = new BigDecimal(Double.toString(v2));
		return b1.multiply(b2).doubleValue();
	}

	/**
	 * 提供（相对）精确的除法运算，当发生除不尽的情况时，精确到 小数点以后10位，以后的数字四舍五入。
	 * @param v1 被除数
	 * @param v2 除数
	 * @return 两个参数的商
	 */
	public static double div(double v1, double v2) {
		return div(v1, v2, 10);
	}

	/**
	 * 提供（相对）精确的除法运算。当发生除不尽的情况时，由scale参数指 定精度，以后的数字四舍五入。
	 * @param v1 被除数
	 * @param v2 除数
	 * @param scale 表示表示需要精确到小数点以后几位。
	 * @return 两个参数的商
	 */
	public static double div(double v1, double v2, int scale) {
		if (scale < 0) {
			throw new IllegalArgumentException("The scale must be a positive integer or zero");
		}
		BigDecimal b1 = new BigDecimal(Double.toString(v1));
		BigDecimal b2 = new BigDecimal(Double.toString(v2));
		return b1.divide(b2, scale, BigDecimal.ROUND_HALF_UP).doubleValue();
	}

	/**
	 * 格式化双精度，保留两个小数
	 * @return
	 */
	public static String formatDouble(Double b) {
		BigDecimal bg = new BigDecimal(b);
		return bg.setScale(2, BigDecimal.ROUND_HALF_UP).toString();
	}

	/**
	 * 百分比计算
	 * @return
	 */
	public static String formatScale(double one, long total) {
		BigDecimal bg = new BigDecimal(one * 100 / total);
		return bg.setScale(0, BigDecimal.ROUND_HALF_UP).toString();
	}
	
	/**
	 * 格式化数值类型
	 * @param data
	 * @param pattern
	 */
	public static String formatNumber(Object data, String pattern) {
		DecimalFormat df = null;
		if (pattern == null) {
			df = new DecimalFormat();
		} else {
			df = new DecimalFormat(pattern);
		}
		return df.format(data);
	}


	
}

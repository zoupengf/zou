package com.keytop.commons.utils;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
import java.util.GregorianCalendar;

public class DateUtils {

	public static final String PATTERN_MONTH = "yyyy-MM";
	public static final String PATTERN_DATE = "yyyy-MM-dd";
	public static final String PATTERN_TIME = "yyyy-MM-dd HH:mm:ss";
	public static final String PATTERN_TIME_MINUTE = "yyyy-MM-dd HH:mm";

    /**
     * 获取当前年
     * @return
     */
	public static int getCurrentYear(){
		Calendar cal = Calendar.getInstance();
		return cal.get(Calendar.YEAR);
	}
	
	/**
	 * 日期转字符串
	 * 
	 * @param date
	 * @param pattern
	 * @return
	 */
	public static String formatDate(Date date, String pattern) {
		if (date == null)
			return "";
		if (pattern == null)
			pattern = PATTERN_DATE;
		SimpleDateFormat simpleDateFormat = new SimpleDateFormat(pattern);
		return simpleDateFormat.format(date);
	}

	/**
	 * 日期转yyyy-MM-dd字符串
	 * 
	 * @param date
	 * @return
	 */
	public static String formatDate(Date date) {
		if (date == null)
			return "";
		return formatDate(date, PATTERN_DATE);
	}

	/**
	 * 日期转yyyy-MM-dd HH:mm:ss字符串
	 * 
	 * @param date
	 * @return
	 */
	public static String formatDateTime(Date date) {
		return formatDate(date, PATTERN_TIME);
	}

	/**
	 * yyyy-MM-dd格式字符串转日期
	 * 
	 * @param dateStr
	 * @param pattern
	 * @return
	 */
	public static Date parseDate(String dateStr, String pattern) {
		try {
			if ((dateStr == null) || (dateStr.equals(""))) {
				return null;
			}

			if (pattern == null) {
				pattern = PATTERN_DATE;
			}
			SimpleDateFormat simpleDateFormat = new SimpleDateFormat(pattern);
			return simpleDateFormat.parse(dateStr);
		} catch (Exception e) {
		}
		return null;
	}

	/**
	 * yyyy-MM-dd HH:mm:ss格式字符串转日期
	 * 
	 * @param dateStr
	 * @return
	 */
	public static Date parseDateTime(String dateStr) {
		try {
			return parseDate(dateStr, PATTERN_TIME);
		} catch (Exception e) {
		}
		return null;
	}

	public static Date add(Date date, int field, int amount) {
		if (date == null) {
			date = new Date();
		}

		Calendar cal = Calendar.getInstance();
		cal.setTime(date);
		cal.add(field, amount);

		return cal.getTime();
	}

	/**
	 * 指定天数（前）后的时间
	 * 
	 * @param date
	 * @param amount
	 * @return
	 */
	public static Date addDay(Date date, int amount) {
		return add(date, Calendar.DAY_OF_MONTH, amount);
	}

	/**
	 * 指定月数（前）后的时间
	 * 
	 * @param date
	 * @param amount
	 * @return
	 */
	public static Date addMonth(Date date, int amount) {
		return add(date, Calendar.MONTH, amount);
	}

	/**
	 * 指定年数（前）后的时间
	 * 
	 * @param date
	 * @param amount
	 * @return
	 */
	public static Date addYear(Date date, int amount) {
		return add(date, Calendar.YEAR, amount);
	}

	/**
	 * 当月月初
	 * 
	 * @param date
	 * @return
	 */
	public static Date getFirstDay(Date date) {
		Calendar cal = Calendar.getInstance();
		cal.setTime(date);
		cal.set(Calendar.DAY_OF_MONTH, 1);
		return cal.getTime();
	}

	/**
	 * 当月月末
	 * 
	 * @param date
	 * @return
	 */
	public static Date getLastDay(Date date) {
		Calendar cal = Calendar.getInstance();
		cal.setTime(date);
		cal.add(Calendar.MONTH, 1);
		cal.set(Calendar.DAY_OF_MONTH, 1);
		cal.add(Calendar.DAY_OF_MONTH, -1);
		return cal.getTime();
	}
	/**
	 * 获取年月日
	 * @param cal
	 * @return
	 */
	public static String getYMDString(Calendar cal)
	{
		if (cal == null)
			return null;
		return new SimpleDateFormat("yyyyMMdd").format(cal.getTime());
		
	}
	/**
	 * 获取到当天凌晨时间 2018-1-1获取到的就是2018-1-1 00:00:00
	 * @return
	 */
	public static Date getTimeOf12() {
        Calendar cal = Calendar.getInstance();
        cal.setTime(new Date());
        cal.set(Calendar.HOUR_OF_DAY, 0);
        cal.set(Calendar.MINUTE, 0);
        cal.set(Calendar.SECOND, 0);
        return  cal.getTime();
    }

	/**
	 * 获取现在往前推天数的凌晨
	 * @return
	 */
	public static Date getTimeOf00(int day) {
		Calendar cal = Calendar.getInstance();
		cal.setTime(new Date());
		cal.set(Calendar.HOUR_OF_DAY, -(24*day));
		cal.set(Calendar.MINUTE, 0);
		cal.set(Calendar.SECOND, 0);
		cal.set(Calendar.MILLISECOND, 0);
		cal.add(Calendar.DAY_OF_MONTH, 1);
		return  cal.getTime();
	}
	/**
	 * 获取两个日期相差天数
	 * @param calendarOfStart
	 * @param calendarOfEnd
	 * @return
	 */
	public static int getDayNumBetween2Date(Calendar calendarOfStart, Calendar calendarOfEnd){
		if (calendarOfStart == null || calendarOfEnd == null)
			return -1;
		Calendar startCal = Calendar.getInstance();
		Calendar toCal = Calendar.getInstance();
		startCal.setTimeInMillis(calendarOfStart.getTimeInMillis());
		toCal.setTimeInMillis(calendarOfEnd.getTimeInMillis());

		startCal.set(Calendar.HOUR, 00);
		startCal.set(Calendar.MINUTE, 00);
		startCal.set(Calendar.SECOND, 000);
		toCal.set(Calendar.HOUR, 00);
		toCal.set(Calendar.MINUTE, 00);
		toCal.set(Calendar.SECOND, 000);
		long dateRange = toCal.getTimeInMillis() - startCal.getTimeInMillis();
		long oneDay = 1000 * 3600 * 24;
		int diffdays = (int) (dateRange / oneDay);
		return diffdays;
	}

    /**
     * 计算两个日期间的月份查（不考虑日期差）
     * @param calendarBegin
     * @param calendarEnd
     * @return
     */
	public static int getMonthsWithNoDay(Calendar calendarBegin, Calendar calendarEnd){
	    return ( calendarEnd.get(Calendar.YEAR) - calendarBegin.get(Calendar.YEAR) ) * 12
            + ( calendarEnd.get(Calendar.MONTH) - calendarBegin.get(Calendar.MONTH) );
    }

	/**
     * 获取过去的天数
     *
     * @param date
     * @return
     */
    public static long pastDays(Date date) {
        long t = new Date().getTime() - date.getTime();
        return t / (24 * 60 * 60 * 1000);
    }

    public static long futureDays(String date) throws Exception{
    	SimpleDateFormat format = new SimpleDateFormat(PATTERN_TIME);
    	Date  currdate = format.parse(date);
        long t =  currdate.getTime() - new Date().getTime();
        return t / (24 * 60 * 60 * 1000);
    }

	/**
	 * 获取指定日期23点59分59秒
	 *
	 * @param date
	 * @return
	 */
	public static Date getTimeOf24(Date date){
    	Calendar calendar = Calendar.getInstance();
		calendar.setTime(date);
		calendar.set(Calendar.HOUR_OF_DAY,23);
		calendar.set(Calendar.MINUTE,59);
		calendar.set(Calendar.SECOND,59);
		return calendar.getTime();
	}

	public static Date getTimeOf24(String src,String sdf) throws ParseException {
		Date date = new SimpleDateFormat(sdf).parse(src);
		return getTimeOf24(date);
	}

	/**
	 * 获取指定日期0点0分0秒
	 *
	 * @param date
	 * @return
	 */
	public static Date getTimeOf00(Date date){
		Calendar calendar = Calendar.getInstance();
		calendar.setTime(date);
		calendar.set(Calendar.HOUR_OF_DAY,0);
		calendar.set(Calendar.MINUTE,0);
		calendar.set(Calendar.SECOND,0);
		return calendar.getTime();
	}

	public static Date getTimeOf00(String src,String sdf) throws ParseException {
		Date date = new SimpleDateFormat(sdf).parse(src);
		return getTimeOf00(date);
	}


	public static String[] getBeginEndDatetime(String datetime) {
		String[] datetimeArray = new String[2];
		datetimeArray[0] = datetime.substring(0, 19);
		datetimeArray[1] = datetime.substring(22);
		return datetimeArray;
	}

	public static String[] getBeginEndDate(String date) {
		String[] dateArray = new String[2];
		dateArray[0] = date.substring(0, 10);
		dateArray[1] = date.substring(12);
		return dateArray;
	}

	/**
	 * 获取Calendar对象
	 * @param dateStr 时间字符串
	 * @param pattern 指定的时间格式
	 * @return
	 */
	public static Calendar getCalendar(String dateStr, String pattern){
		Date date = parseDate(dateStr, pattern);
		return getCalendar(date);
	}

    /**
     * 获取Calendar
     * @param date 时间
     * @return
     */
    public static Calendar getCalendar(Date date){
        Calendar calendar = Calendar.getInstance();
        calendar.setTime(date);
        return calendar;
    }

	/**
	 * 获取yyyy-MM-dd 格式的日期字符串
	 * @param calendar
	 * @return
	 */
	public static String getDateStr(Calendar calendar){
		int month = calendar.get(Calendar.MONTH) + 1;
		int day = calendar.get(Calendar.DAY_OF_MONTH);

		return calendar.get(Calendar.YEAR) + "-" + (month < 10 ? "0" : "") + month + "-" + (day < 10 ? "0" : "") + day;
	}

	/**
	 * 得到  上周 周五日期 MM-dd 例如： 06-01
	 * @return
	 */
	public static String getWeekFridayFormat(){
		SimpleDateFormat formater=new SimpleDateFormat("MM-dd");
		Calendar calendar = Calendar.getInstance();
		calendar.add(Calendar.WEEK_OF_YEAR, -1);
		calendar.set(Calendar.DAY_OF_WEEK,6);
		Date first=calendar.getTime();
		return formater.format(first);
	}

	/**
	 * 得到上周 周五日期
	 * @return
	 */
	public static String getWeekFriday(){
		SimpleDateFormat formater=new SimpleDateFormat("yyyy-MM-dd");
		Calendar calendar = Calendar.getInstance();
		calendar.add(Calendar.WEEK_OF_YEAR, -1);
		calendar.set(Calendar.DAY_OF_WEEK,6);
		Date first=calendar.getTime();
    return formater.format(first);
	}

	/**
	 * 得到本周周一日期
	 * @return
	 */
	public static String getWeekMonday(){
		SimpleDateFormat formater=new SimpleDateFormat("yyyy-MM-dd");
		Calendar cal=new GregorianCalendar();
		cal.setFirstDayOfWeek(Calendar.MONDAY);
		cal.setTime(new Date());
		cal.set(Calendar.DAY_OF_WEEK, cal.getFirstDayOfWeek());
		Date first=cal.getTime();
		return formater.format(first);
	}

	/**
	 * 获取 上 上周六 的日期
	 * @return
	 */
	public static String getLastWeekSaturday() {
		SimpleDateFormat formater=new SimpleDateFormat("yyyy-MM-dd");
		Calendar calendar = Calendar.getInstance();
		calendar.add(Calendar.DAY_OF_MONTH,-9);
		Date first=calendar.getTime();
		return formater.format(first);
	}

	/**
	 * 得到传入时间的 晚上18：00:00
	 * 2019-06-14 18:00:00
	 * @return
	 */
	public static String getNightSixClock(String time) {
		SimpleDateFormat formater=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		Date date =  DateUtils.parseDate(time,"yyyy-MM-dd");
		Calendar calendar = Calendar.getInstance();
		calendar.setTime(date);
		//calendar.add(Calendar.WEEK_OF_YEAR, -1);
		//calendar.set(Calendar.DAY_OF_WEEK, 6);
		calendar.set(Calendar.HOUR_OF_DAY, 18);
		calendar.set(Calendar.MINUTE, 0);
		calendar.set(Calendar.SECOND, 0);
		Date date1 = calendar.getTime();
		return formater.format(date1);
	}

	/**
	* @Author: zoupengfei
	* @Description: 时间戳 转成 对应的格式  yyyy-MM-dd HH:mm:ss
	* @Date: 2019/9/2
	* @Param: [str]
	* @return: java.lang.String
	* @throws:
	*/
	public static String stampToDate(String str){
		SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		long lt = new Long(str);
		Date date = new Date(lt);
		return simpleDateFormat.format(date);
	}
}

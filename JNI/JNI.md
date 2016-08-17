
#JNI中相互调用
---
##java调用c/c++生成的dll库.

 -  在java中编写native方法
 -  javac  , javah  生成头文件
 -  编辑源文件,针对头文件,用c/c++以及JNI中的函数实现方法,  
 -  生成dll文件,放在配置了环境变量的文件夹中
 -  在java文件中 loadlibrary直接调用



##c/c++调用java属性以及方法.

   调用遵循一个规律:
    
    在c文件中,实现相应方法(JNIEnv *env, jobject jobj),
    流程为(具体实现不尽相同):
    1.jclass cls = (*env)->GetObjectClass(env, jobj);
    2.jfieldID fid = (*env)->GetFieldID(env, cls, "key", "Ljava/lang/String;");
      其中的属性/方法名,以及签名可变,签名从javap -s -p class完整路径类名 得到.
    3.jint jit=(*env)->GetStaticIntField(env, jcs, fid);  //属性
     //调用//CallStatic<Type>Method
     jstring uuid = (*env)->CallStaticObjectMethod(env, cls, mid);
    4.对JNI类型进行操作
    
    5.如果带有返回值,则需要再将内容返回(c->jni) 
        //修改k//Set<Type>Field 
        (*env)->SetObjectField(env, jobj, fid, new_jstr);
	
        

##完整源文件
 - ###相关java文件
 
```java
package com.ccj.jni;

import java.util.Random;
import java.util.UUID;



public class JniDemo {
    
	public String key = "ccj";
	
	public static int count = 9;
	
	public native static String getStringFromC();
	
	public native String getStringFromC2(int i);
	//访问属性，返回修改之后的属性内容
	public native String accessField();
	
	public native void accessStaticField();
	
	public native void accessMethod();
	
	public native void accessStaticMethod();
	
	public static void main(String[] args) {
		String text = getStringFromC();
		System.out.println(text);
		JniDemo t = new JniDemo();
		text = t.getStringFromC2(6);
		System.out.println(text);
		
		System.out.println("key修改前："+t.key);
		t.accessField();
		System.out.println("key修改后："+t.key);
		
		System.out.println("count修改前："+count);
		t.accessStaticField();
		System.out.println("count修改后："+count);
		
		t.accessMethod();
		t.accessStaticMethod();
	}
	
	//产生指定范围的随机数
	public int genRandomInt(int max){
		System.out.println("genRandomInt 执行了...");
		return new Random().nextInt(max); 
	}
	
	//产生UUID字符串
	public static String getUUID(){
		return UUID.randomUUID().toString();
	}
	
	//加载动态库
	static{	
		System.loadLibrary("Project5");
	}
	
}


```

 - ###相关c文件


```c

#define _CRT_SECURE_NO_WARNINGS
#include "com_ccj_jni_JniDemo.h"
#include <string.h>
#include <stdlib.h>
#include <string.h>

/**********************java调用C********************************/
/*
* Class:     com_ccj_jni_JniDemo
* Method:    getStringFromC java调用C
* Signature: ()Ljava/lang/String;
*/
JNIEXPORT jstring JNICALL Java_com_ccj_jni_JniDemo_getStringFromC
(JNIEnv * env, jclass jcs){
    
	return (*env)->NewStringUTF(env, "String from c");

}


/*
* Class:     com_ccj_jni_JniDemo
* Method:    getStringFromC2
* Signature: (I)Ljava/lang/String;
*/
JNIEXPORT jstring JNICALL Java_com_ccj_jni_JniDemo_getStringFromC2
(JNIEnv *env, jobject job, jint jint){
		int num = jint + 2;
		char *temp = "num is:";
		//strcat(temp,num);
		return (*env)->NewStringUTF(env, temp);
		

};
/**********************C调用java********************************/
/*
* Class:     com_ccj_jni_JniDemo
* Method:    accessField 调用属性key,并修改
* 方法:fid->field->变为char->用c修改->jstring->Set<Type>Field 
* Signature: ()Ljava/lang/String;
*/
JNIEXPORT jstring JNICALL Java_com_ccj_jni_JniDemo_accessField
(JNIEnv *env, jobject jobj){
		//jobj是t对象，JniTest.class
		jclass cls = (*env)->GetObjectClass(env, jobj);
		//jfieldID
		//属性名称，属性签名
		jfieldID fid = (*env)->GetFieldID(env, cls, "key", "Ljava/lang/String;");

		//jason >> super jason
		//获取key属性的值
		//Get<Type>Field
		jstring jstr = (*env)->GetObjectField(env, jobj, fid);
		printf("jstr:%#x\n", &jstr);


		//jstring -> c字符串
		//isCopy 是否复制（true代表赋值，false不复制）
		char *c_str = (*env)->GetStringUTFChars(env, jstr, JNI_FALSE);
		//拼接得到新的字符串
		char text[20] = "super ";
		strcat(text, c_str);

		//c字符串 ->jstring
		jstring new_jstr = (*env)->NewStringUTF(env, text);

		//修改key
		//Set<Type>Field
		(*env)->SetObjectField(env, jobj, fid, new_jstr);

		printf("new_jstr:%#x\n", &new_jstr);

		return new_jstr;

}

/*
* Class:     com_ccj_jni_JniDemo
* Method:    accessStaticField 修改Java中count值
* Signature: ()V
*/
JNIEXPORT void JNICALL Java_com_ccj_jni_JniDemo_accessStaticField
(JNIEnv *env, jobject job){
	jclass jcs=(*env)->GetObjectClass(env,job);
	jfieldID fid = (*env)->GetStaticFieldID(env, jcs, "count", "I");

	jint jit=(*env)->GetStaticIntField(env, jcs, fid);

	jit++;
	//printf("niDemo_accessStaticField:%s\n", jit);
	(*env)->SetStaticIntField(env, jcs, fid, jit);

};

/*
* Class:     com_ccj_jni_JniDemo
* Method:    accessMethod
* Signature: ()V
*/
JNIEXPORT void JNICALL Java_com_ccj_jni_JniDemo_accessMethod
(JNIEnv *env, jobject job){

	jclass jcs = (*env)->GetObjectClass(env, job);
	jfieldID fid = (*env)->GetMethodID(env, jcs, "genRandomInt", "(I)I");
	jint jit=(*env)->CallIntMethod(env, job, fid);

	printf("Java_com_ccj_jni_JniDemo_accessMethod%s\n", jit);



};

/*
* Class:     com_ccj_jni_JniDemo
* Method:    accessStaticMethod 调用方法对filefile进行操作
* Signature: ()V
*/
JNIEXPORT void JNICALL Java_com_ccj_jni_JniDemo_accessStaticMethod
(JNIEnv *env, jobject job){
	//jclass
	jclass cls = (*env)->GetObjectClass(env, job);
	//jmethodID	
	jmethodID mid = (*env)->GetStaticMethodID(env, cls, "getUUID", "()Ljava/lang/String;");

	//调用
	//CallStatic<Type>Method
	jstring uuid = (*env)->CallStaticObjectMethod(env, cls, mid);
	
	char * uuid_str = (*env)->GetStringUTFChars(env,uuid,NULL);

	char *fileName[100];

	sprintf(fileName, "D:\\%s.txt", uuid_str);

	FILE *file = fopen(fileName,"w");
	fputs("Hellow Method",file);
	fclose(file);
};


```

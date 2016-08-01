---
layout:     post
title:      CRUD Android dengan Volley
date:       2016-07-29 22:46:00
summary:    Assalamualaikum Wr.Wb. Halo, Berikut tutorial cara melakukan CRUD di Android dengan menggunakan json dan librari volley. Tutorial kali ini saya tidak akan bahas secara spesifik mengenai penjelasan dibeberapa komponen seperti json, webservices, dsb., jadi saya sarankan baca terlebih dahulu mengenai hal-hal tersebut.
categories: Android
---

Assalamualaikum Wr.Wb. Halo, Berikut tutorial cara melakukan CRUD di Android dengan menggunakan json dan librari volley. Tutorial kali ini saya tidak akan bahas secara spesifik mengenai penjelasan dibeberapa komponen seperti json, webservices, dsb., jadi saya sarankan baca terlebih dahulu mengenai hal-hal tersebut. Sebelum develop, hal yang biasa saya lakukan yaitu membuat rancangan seperti arsitektur database, mockup aplikasi, dan sebagainya. 

Untuk arsitektur databasenya, seperti ini:
![desk]()

Dan kurang lebih tampilannya akan seperti ini (ini hanya mockup, `nanti di tutorial selanjutnya saya akan share cara membuat mockup di design editor`):
![desk]()

Sekarang, buat sebuah project baru di android studio dengan nama dan company domain bebas. Untuk minimum SDK pilih API Kitkat saja. Nanti hasil akhir struktur project kita seperti ini:
![desk]()

Nah, pertama-tama kita harus tambahkan sebuah library ([gradle](https://en.wikipedia.org/wiki/Gradle)) di project kita.
pertama, volley.
`'com.mcxiaoke.volley:library:1.0.19'`, library ini berfungsi sebagai HTTP Request. Kita dapat melakukan `POST` ataupun `GET` pada layanan web services kita.
kedua, recyclerview.
`'com.android.support:recyclerview-v7:23.3.0'`, library ini sama seperti listview, tapi recyclerview ini lebih customize dan teknologi baru yang dibuat oleh Google sebagai pengganti listview.
ketiga, cardview.
`'com.android.support:cardview-v7:23.3.0'`, library ini sebagai listitem di recyclerview.
keempat, gson.
`'com.google.code.gson:gson:2.6.1'`, library ini sebagai converter json kedalam bentuk objek.
Setelah library ditambahkan, pertama-tama kita melakukan design layout dulu.

Untuk activity_main.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="app.isfaaghyth.belajarcrud.MainActivity"
    android:background="#fff">

    <android.support.v7.widget.RecyclerView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/rv_item"/>
</RelativeLayout>
{% endhighlight %}

Untuk activity_detail.xml
{% highlight xml %}
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <RelativeLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">

        <RelativeLayout
            android:layout_width="match_parent"
            android:layout_height="200dp"
            android:id="@+id/relative_img">

            <ImageView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:id="@+id/img_avatar"
                android:layout_centerVertical="true"
                android:layout_centerHorizontal="true"
                android:padding="10dp"
                android:src="@mipmap/ic_launcher"/>

        </RelativeLayout>

        <RelativeLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_below="@+id/relative_img"
            android:id="@+id/relative_cardview"
            android:layout_marginRight="10dp"
            android:layout_marginLeft="10dp">

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/cardview_name"
                android:clickable="true"
                app:cardElevation="0dp"
                android:foreground="?android:attr/selectableItemBackground">
                <RelativeLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:padding="15dp">
                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/txt_name"
                        android:text="Full Name"
                        android:textStyle="bold"/>
                </RelativeLayout>
            </android.support.v7.widget.CardView>

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_below="@+id/cardview_name"
                android:id="@+id/cardview_office"
                android:layout_marginTop="10dp"
                android:clickable="true"
                app:cardElevation="0dp"
                android:foreground="?android:attr/selectableItemBackground">
                <RelativeLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:padding="15dp">
                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/txt_office"
                        android:text="Office"
                        android:textStyle="bold"/>
                </RelativeLayout>
            </android.support.v7.widget.CardView>

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_below="@+id/cardview_office"
                android:id="@+id/cardview_email"
                android:layout_marginTop="10dp"
                android:clickable="true"
                app:cardElevation="0dp"
                android:foreground="?android:attr/selectableItemBackground">
                <RelativeLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:padding="15dp">
                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/txt_email"
                        android:text="Email@email.com"
                        android:textStyle="bold"/>
                </RelativeLayout>
            </android.support.v7.widget.CardView>
        </RelativeLayout>

        <RelativeLayout
            xmlns:android="http://schemas.android.com/apk/res/android"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:layout_below="@+id/relative_cardview"
            android:layout_marginTop="20dp"
            android:layout_marginRight="10dp"
            android:layout_marginLeft="10dp">

            <Button
                android:id="@+id/btnDelete"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:background="@color/colorAccent"
                android:textColor="#fff"
                android:layout_alignParentLeft="true"
                android:layout_alignTop="@+id/btnModify"
                android:layout_toLeftOf="@+id/view"
                android:text="DELETE" />

            <View
                android:id="@+id/view"
                android:layout_width="10dp"
                android:layout_height="1dp"
                android:layout_centerHorizontal="true" />

            <Button
                android:id="@+id/btnModify"
                android:layout_width="wrap_content"
                android:background="@color/cardview_dark_background"
                android:textColor="#fff"
                android:layout_height="wrap_content"
                android:layout_alignParentRight="true"
                android:layout_toRightOf="@+id/view"
                android:text="MODIFY" />
        </RelativeLayout>

    </RelativeLayout>

</ScrollView>
{% endhighlight %}

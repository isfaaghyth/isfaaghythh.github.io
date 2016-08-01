---
layout:     post
title:      CRUD Android dengan Volley
date:       2016-07-29 22:46:00
summary:    Assalamualaikum Wr.Wb. Halo, Berikut tutorial cara melakukan CRUD di Android dengan menggunakan json dan librari volley. Tutorial kali ini saya tidak akan bahas secara spesifik mengenai penjelasan dibeberapa komponen seperti json, webservices, dsb., jadi saya sarankan baca terlebih dahulu mengenai hal-hal tersebut.
categories: Android
---

Assalamualaikum Wr.Wb. Halo, Berikut tutorial cara melakukan CRUD di Android dengan menggunakan json dan librari volley. Tutorial kali ini saya tidak akan bahas secara spesifik mengenai penjelasan dibeberapa komponen seperti json, webservices, dsb., jadi saya sarankan baca terlebih dahulu mengenai hal-hal tersebut. Sebelum develop, hal yang biasa saya lakukan yaitu membuat rancangan seperti arsitektur database, mockup aplikasi, dan sebagainya. 

Untuk arsitektur databasenya, seperti ini:

![desk](https://s19.postimg.org/i617g9or7/image.jpg)

Dan kurang lebih tampilannya akan seperti ini (ini hanya mockup, `nanti di tutorial selanjutnya saya akan share cara membuat mockup di design editor`):

![desk](https://s19.postimg.org/niq1uecnn/image.jpg)

Sekarang, buat sebuah project baru di android studio dengan nama dan company domain bebas. Untuk minimum SDK pilih API Kitkat saja. Nanti hasil akhir struktur project kita seperti ini:

![desk](https://s19.postimg.org/jc57f2d1v/struktur.png)

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

Untuk dialog_add.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin" >

    <EditText
        android:id="@+id/txtName"
        android:hint="Name"
        android:padding="10dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background_normal"/>

    <EditText
        android:id="@+id/txtOffice"
        android:hint="Office"
        android:padding="10dp"
        android:layout_marginTop="5dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background_normal"/>

    <EditText
        android:id="@+id/txtEmail"
        android:hint="Email"
        android:padding="10dp"
        android:layout_marginTop="5dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background_normal"/>

</LinearLayout>
{% endhighlight %}

Untuk layout_item.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/cardview_item"
        android:clickable="true"
        app:cardElevation="0dp"
        android:foreground="?android:attr/selectableItemBackground">

    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="12dp">

        <ImageView
            android:layout_width="50dp"
            android:layout_height="50dp"
            android:id="@+id/img_avatar"
            android:src="@mipmap/ic_launcher"
            android:layout_marginRight="15dp"
            android:layout_centerVertical="true"/>

        <RelativeLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_toRightOf="@+id/img_avatar"
            android:layout_centerVertical="true">

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/txt_name"
                android:text="Full Name"
                android:textStyle="bold"/>

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/txt_office"
                android:layout_below="@+id/txt_name"
                android:text="Tittle"/>

        </RelativeLayout>
    </RelativeLayout>
    </android.support.v7.widget.CardView>
</RelativeLayout>
{% endhighlight %}

Oke, untuk layoutnya sudah dibuat. sekarang, kita masuk ke sisi backend nya. Untuk sisi backend nya sendiri, saya hanya menjelaskan beberapa hal yang penting saja. sisanya, silahkan fork projectnya ya.

Untuk sisi backend sendiri, saya buat sangat sederhana dan sangat mudah dipahami. Silahkan perhatikan kode berikut:

untuk file show.php, menampilkan data dari database dalam bentuk json.
{% highlight php %}
<?php  
	$username ="isfa";
	$password ="";
	$hostname = "localhost";
	$database_name = "daengid_daengbelajar";
	
	$con = mysqli_connect($hostname , $username, $password);
	$selected = mysqli_select_db($con, $database_name);

	$result = mysqli_query($con, "SELECT * FROM db_user");
	
	$json_response = array();
	
	while ($row = mysqli_fetch_array($result, MYSQLI_ASSOC)) {
		$json_response[] = $row;
	}
			
	echo json_encode(array('belajar' => $json_response));  

?>
{% endhighlight %}

untuk file modify.php, memodifikasi data yang sudah ada.
{% highlight php %}
<?php  
	$username ="isfa";
	$password ="";
	$hostname = "localhost";
	$database_name = "daengid_daengbelajar";
	
	$con = mysqli_connect($hostname , $username, $password);
	$selected = mysqli_select_db($con, $database_name);

	$data[0] = $_GET['id'];
	$data[1] = $_GET['name'];
	$data[2] = $_GET['office'];
	$data[3] = $_GET['email'];

	$result = mysqli_query($con, "UPDATE db_user SET name='$data[1]', office='$data[2]', email='$data[3]' WHERE id='$data[0]'");

	if ($result) {
	echo "sukses";
	} else {
	echo "gagal";
	}
?>
{% endhighlight %}

untuk file insert.php, menambahkan data kedalam database.
{% highlight php %}
<?php  
	$username ="isfa";
	$password ="";
	$hostname = "localhost";
	$database_name = "daengid_daengbelajar";
	
	$con = mysqli_connect($hostname , $username, $password);
	$selected = mysqli_select_db($con, $database_name);

	$data[0] = $_GET['name'];
	$data[1] = $_GET['office'];
	$data[2] = $_GET['email'];

	$result = mysqli_query($con, "INSERT INTO db_user(name,office,email) VALUES('$data[0]','$data[1]','$data[2]')");

	if ($result) {
	echo "sukses";
	} else {
	echo "gagal";
	}
?>
{% endhighlight %}

untuk file insert.php, menambahkan data kedalam database.
{% highlight php %}
<?php  
	$username ="isfa";
	$password ="";
	$hostname = "localhost";
	$database_name = "daengid_daengbelajar";
	
	$con = mysqli_connect($hostname , $username, $password);
	$selected = mysqli_select_db($con, $database_name);

	$id = $_GET['id'];

	$result = mysqli_query($con, "DELETE FROM db_user WHERE id='$id'");

	if ($result) {
	echo "sukses";
	} else {
	echo "gagal";
	}
?>
{% endhighlight %}

Lalu bagaimana integrasi Library volley nya untuk bisa `GET` / `POST` dari API ? Nah, untuk penggunaan volley nya, kita membutuhkan sebuah fungsi dengan tipe data `RequestQueue` dan `StringRequest`. `RequestQueue` sebagai context handling, dan `StringRequest` sebagai Web Request nya. 

Lihat kode dibawah ini:
{% highlight java %}
        RequestQueue queue = Volley.newRequestQueue(getApplicationContext());
        StringRequest stringRequest = new StringRequest(Request.Method.GET, "http://urlnya.com/file.php?id=x",
                new Response.Listener<String>() {;
            @Override
            public void onResponse(String response) {
                Log.d("Belajar", response);
                // diproses disini >> response
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                Log.e("Belajar", error.toString());
            }
        });
        queue.add(stringRequest);
{% endhighlight %}

`StringRequest` mempunyai 3 buah parameter, yang pertama sebagai metode proses API nya, lalu parameter kedua adalah URL Web service nya, dan yang ketiga sebagai Response Listener. Output yang dihasilakn dari Request API dapat dilihat di sub prosedur `onResponse` dengan mengambil dari parameter _response_.

Sebagai contoh, saya sudah siapkan data dan web services nya di sharedhost saya. dan output jsonnya seperti ini:

![desk](https://s19.postimg.org/jav9lnb83/json_output.png)

Dari output tersebut, kita bisa lihat bahwa datanya berada didalam array 'belajar'. Maka, untuk mengonversi kedalam bentuk object menggunakan gson, kita membutuhkan kode seperti dibawah ini:

{% highlight java %}
    public class ObjectBelajar {
        @SerializedName("belajar")
        public List<Results> belajar;

        public class Results {
            @SerializedName("id")
            public String id;

            @SerializedName("name")
            public String name;

            @SerializedName("office")
            public String office;

            @SerializedName("email")
            public String email;
        }
    }
{% endhighlight %}

Lalu bagaimana caranya kita mengirim data dari `activity1` ke `activity2` ? Caranya cukup mudah. kita menggunakan _putExtra_ dan _getExtra_ pada Intent. Seperti berikut saya implementasikan didalam `onBindViewHolder` pada cardview yang digunakan didalam recyclerview.

{% highlight java %}
        i.putExtra("id", resultsList.get(position).id);
        i.putExtra("name", resultsList.get(position).name);
        i.putExtra("office", resultsList.get(position).office);
        i.putExtra("email", resultsList.get(position).email);
{% endhighlight %}

Untuk array `resultsList` berasal dari class gson yang sudah kita buat sebelumnya, lalu mengambil data sesuai nama object dari json tersebut.

[Fork in here](https://github.com/isfaaghyth/CRUD-Android/)

Nah, mungkin cuman sedikit aja yang bisa saya jelaskan. Bagi yang kesulitan silahkan di fork projectnya dan berkomentar.

stay and keep coding. :)

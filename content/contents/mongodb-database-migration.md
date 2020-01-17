---
title: "MongoDB Database Migration"
date: 2013-04-16 02:13:05+07:00
publishDate: 2013-04-16 02:13:05+07:00
url: "/content/mongodb-database-migration"
tags: ["MongoDB", "database"]
draft: false
---

{{< figure src="/images/2013/MongoDB_Logo.png" title="MongoDB Database Migration" >}}

Suatu saat ada kebutuhan migrasi database server MongoDB dari satu datacenter ke datacenter lain.

Setelah mencari jawabannya di Google, ternyata caranya cukup mudah, cukup matikan proses mongod dan pindahkan direktori database tersebut ke server lain.

Berikut dua metode yang cukup mudah diambil dari [1]

Metode 1 (termudah, dapat menyebabkan downtime cukup lama)

<ul>
	<li>shutdown mongod</li>
	<li>copy direktori yang ada di "dbpath" ke mesin yang baru</li>
	<li>nyalakan mongod di mesin yang baru dengan variabel path yang sama</li>
</ul>

Metode 2 (migrasi lebih lama, downtime lebih sedikit)

<ul>
	<li>stop dan restart mongod sebagai master (jika sebelumnya belum jadi master)</li>
	<li>install mongod di mesin yang baru dan konfigurasi menjadi slave</li>
	<li>tunggu hingga slave menyalin semua DB, re-index dan tersinkronisasi seluruhnya</li>
	<li>setelah slave sudah tersinkronisasi lakukan langkah-langkah berikut untuk meminimalisasi downtime
	<ul>
		<li>matikan semua akses write ke MongoDB (semua proses yg menuliskan data), jadi saat ini statusnya hanya read-only</li>
		<li>setelah semua akses write berhenti dan semua transaksi yang ditulis sudah commit, restart slave (mesin baru) sebagai master</li>
		<li>arahkan semua akses read ke mesin baru (master baru)</li>
		<li>matikan server lama</li>
		<li>nyalakan kembali ke akses write&nbsp;</li>
	</ul>
	</li>
</ul>

Metode lainnya, menggunakan mekanisme export di mesin yang lama dan import di mesin yang baru [7].

Jika ada problem dalam proses menyala/matikan server bisa mengecek [2], setidaknya gunakan kill -2 PID dibanding kill -9 PID.

[1] https://groups.google.com/forum/?fromgroups#!topic/mongodb-user/gqCd0z89N38

[2] https://github.com/emerleite/mongo-migrate

[3] https://groups.google.com/forum/?fromgroups#!topic/mongodb-user/1-tRRGCTvoA

[4] http://docs.mongodb.org/manual/tutorial/copy-databases-between-instances/

[5] http://docs.mongodb.org/manual/tutorial/manage-mongodb-processes/

[6] http://stackoverflow.com/questions/8495293/whats-a-clean-way-to-stop-mongod-on-mac-os-x

[7] http://docs.mongodb.org/manual/core/import-export/

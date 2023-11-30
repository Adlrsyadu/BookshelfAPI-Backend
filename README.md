# BookshelfAPI-Backend

Deskripsi dari Aplikasi yang saya buat:

1.	Aplikasi menggunakan port 9000
    Aplikasi menggunakan port 9000. Jika komputer yang Anda gunakan tidak bisa memakai port 9000,  buatlah dengan port lain.
2.	Aplikasi dijalankan dengan perintah npm run start.
    Aplikasi memiliki runner script start. Cara membuatnya, dengan menambahkan properti start ke dalam properti scripts pada package.json seperti       berikut:
    {
      "name": "submission",
      ...
      "scripts": {
        "start": "node src/server.js",
      }
    }
    Aplikasi tidak dijalankan dengan menggunakan nodemon. Jika ingin menggunakan nodemon dalam proses development, masukkan nodemon kedalam runner      script lain, contohnya:
    {
      "name": "submission",
      ...
      "scripts": {
        "start": "node src/server.js",
        "start-dev": "nodemon src/server.js",
      }
    }
    

3.	API dapat menyimpan buku
    dapat menyimpan buku melalui route:
    Method : POST
    URL : /books
    Body Request:
    {
        "name": string,
        "year": number,
        "author": string,
        "summary": string,
        "publisher": string,
        "pageCount": number,
        "readPage": number,
        "reading": boolean
    }
  	
    Objek buku yang disimpan pada server memiliki struktur seperti di bawah ini:
    
    {
        "id": "Qbax5Oy7L8WKf74l",
        "name": "Buku A",
        "year": 2010,
        "author": "John Doe",
        "summary": "Lorem ipsum dolor sit amet",
        "publisher": "Dicoding Indonesia",
        "pageCount": 100,
        "readPage": 25,
        "finished": false,
        "reading": false,
        "insertedAt": "2021-03-04T09:11:44.598Z",
        "updatedAt": "2021-03-04T09:11:44.598Z"
    }
    Berikut penjelasannya:
        •	id : nilai id bersifat unik. Untuk membuat nilai unik, bisa memanfaatkan nanoid. Untuk  yang menggunakan CommonJS untuk sistem         modularisasi, pastikan memasang nanoid versi 3 melalui perintah: npm install nanoid@3.
        •	finished : merupakan properti boolean yang menjelaskan apakah buku telah selesai dibaca atau belum. Nilai finished didapatkan dari observasi pageCount === readPage.
        •	insertedAt : merupakan properti yang menampung tanggal dimasukkannya buku. Bisa gunakan new Date().toISOString() untuk menghasilkan nilainya.
        •	updatedAt : merupakan properti yang menampung tanggal diperbarui buku. Ketika buku baru dimasukkan, berikan nilai properti ini sama dengan insertedAt.
    
    Server harus merespons gagal bila:
    Client tidak melampirkan properti namepada request body. Bila hal ini terjadi, maka server akan merespons dengan:
    Status Code : 400
    Response Body:
    {
        "status": "fail",
        "message": "Gagal menambahkan buku. Mohon isi nama buku"
    }
    
    Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, maka server akan merespons dengan:
    Status Code : 400
    Response Body:
    {
        "status": "fail",
        "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"
    }
    
    Bila buku berhasil dimasukkan, server harus mengembalikan respons dengan:
    Status Code : 201
    Response Body:
    {
        "status": "success",
        "message": "Buku berhasil ditambahkan",
        "data": {
            "bookId": "1L7ZtDUFeGs7VlEt"
        }
    }

4.	 API dapat menampilkan seluruh buku
      Menampilkan seluruh buku yang disimpan melalui route:
      Method : GET
      URL: /books
      
      Server harus mengembalikan respons dengan:
      Status Code : 200
      Response Body:
      {
          "status": "success",
          "data": {
              "books": [
                  {
                      "id": "Qbax5Oy7L8WKf74l",
                      "name": "Buku A",
                      "publisher": "Dicoding Indonesia"
                  },
                  {
                      "id": "1L7ZtDUFeGs7VlEt",
                      "name": "Buku B",
                      "publisher": "Dicoding Indonesia"
                  },
                  {
                      "id": "K8DZbfI-t3LrY7lD",
                      "name": "Buku C",
                      "publisher": "Dicoding Indonesia"
                  }
              ]
          }
      }
      
      Jika belum terdapat buku yang dimasukkan, server bisa merespons dengan array books kosong.
      {
          "status": "success",
          "data": {
              "books": []
          }
      }

5.	 API dapat menampilkan detail buku
    Menampilkan seluruh buku yang disimpan melalui route:
    Method : GET
    URL: /books/{bookId}
    Bila buku dengan id yang dilampirkan oleh client tidak ditemukan, maka server harus mengembalikan respons dengan:
    Status Code : 404
    Response Body:
    {
        "status": "fail",
        "message": "Buku tidak ditemukan"
    }
    
    Bila buku dengan id yang dilampirkan ditemukan, maka server harus mengembalikan respons dengan:
    Status Code : 200
    Response Body:
    {
        "status": "success",
        "data": {
            "book": {
                "id": "aWZBUW3JN_VBE-9I",
                "name": "Buku A Revisi",
                "year": 2011,
                "author": "Jane Doe",
                "summary": "Lorem Dolor sit Amet",
                "publisher": "Dicoding",
                "pageCount": 200,
                "readPage": 26,
                "finished": false,
                "reading": false,
                "insertedAt": "2021-03-05T06:14:28.930Z",
                "updatedAt": "2021-03-05T06:14:30.718Z"
            }
        }
    }
6.	 API dapat mengubah data buku
    Dapat mengubah data buku berdasarkan id melalui route:
    Method : PUT
    URL : /books/{bookId}
    Body Request:
    {
        "name": string,
        "year": number,
        "author": string,
        "summary": string,
        "publisher": string,
        "pageCount": number,
        "readPage": number,
        "reading": boolean
    }
    
    Server harus merespons gagal bila:
    Client tidak melampirkan properti name pada request body. Bila hal ini terjadi, maka server akan merespons dengan:
    Status Code : 400
    Response Body:
    {
        "status": "fail",
        "message": "Gagal memperbarui buku. Mohon isi nama buku"
    }
    
    Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, maka server akan merespons dengan:
    Status Code : 400
    Response Body:
    {
        "status": "fail",
        "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"
    }
    
    Id yang dilampirkan oleh client tidak ditemukkan oleh server. Bila hal ini terjadi, maka server akan merespons dengan:
    Status Code : 404
    Response Body:
    {
        "status": "fail",
        "message": "Gagal memperbarui buku. Id tidak ditemukan"
    }
    
    Bila buku berhasil diperbarui, server harus mengembalikan respons dengan:
    Status Code : 200
    Response Body:
    {
        "status": "success",
        "message": "Buku berhasil diperbarui"
    }

7.	 API dapat menghapus buku
    Dapat menghapus buku berdasarkan id melalui route berikut:
    Method : DELETE
    URL: /books/{bookId}
    Bila id yang dilampirkan tidak dimiliki oleh buku manapun, maka server harus mengembalikan respons berikut:
    Status Code : 404
    Response Body:
    
    {
        "status": "fail",
        "message": "Buku gagal dihapus. Id tidak ditemukan"
    }
    
    Bila id dimiliki oleh salah satu buku, maka buku tersebut harus dihapus dan server mengembalikan respons berikut:
    Status Code : 200
    Response Body:
    {
        "status": "success",
        "message": "Buku berhasil dihapus"
    }


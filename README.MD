<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pesan Anonim</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #a894c3, #ff50a8);
      color: white;
    }

    .container {
      max-width: 600px;
      margin: 40px auto;
      background: rgba(0, 0, 0, 0.4);
      border-radius: 15px;
      padding: 25px;
      box-shadow: 0 0 15px rgba(0,0,0,0.4);
    }

    h2 {
      text-align: center;
      margin-bottom: 20px;
      color: #fff;
    }

    label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }

    input, textarea {
      width: 100%;
      border: 2px solid #ed6dc3;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 10px;
      color: white;
      padding: 10px;
      margin-bottom: 15px;
      outline: none;
    }

    input::placeholder, textarea::placeholder {
      color: rgba(255,255,255,0.6);
    }

    input[type="file"] {
      border: none;
    }

    button {
      width: 100%;
      background: linear-gradient(to right, #c6bad9, #ff67b8);
      color: white;
      font-weight: bold;
      border: none;
      border-radius: 10px;
      padding: 12px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      opacity: 0.9;
    }

    .pesan-list {
      margin-top: 30px;
    }

    .pesan-item {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 10px;
      padding: 15px;
      margin-bottom: 15px;
      display: flex;
      align-items: center;
      gap: 15px;
    }

    .pesan-item img {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid #ff66cc;
    }

    .teks {
      flex: 1;
    }

    .nama {
      font-weight: bold;
      color: #ffd6ff;
    }

    .isi {
      margin-top: 5px;
      color: #fff;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Asikaa-secret 💌</h2>

    <label>Nama</label>
    <input type="text" id="nama" placeholder="Masukkan nama anda">

    <label>Pesan</label>
    <textarea id="pesan" placeholder="Masukkan pesan anda..." rows="4"></textarea>

    <label>Foto Profil (Opsional)</label>
    <input type="file" id="foto" accept="image/*">

    <button onclick="kirimPesan()">Kirim Pesan</button>

    <div class="pesan-list" id="pesanList"></div>
  </div>

  <script>
    const pesanList = document.getElementById('pesanList');

    const dataPesan = JSON.parse(localStorage.getItem('pesanAnonim')) || [];

    function tampilkanPesan() {
      pesanList.innerHTML = '';
      dataPesan.forEach(p => {
        const div = document.createElement('div');
        div.className = 'pesan-item';
        div.innerHTML = `
          <img src="${p.foto || 'https://via.placeholder.com/50'}">
          <div class="teks">
            <div class="nama">${p.nama || 'Anonim'}</div>
            <div class="isi">${p.pesan}</div>
          </div>
        `;
        pesanList.appendChild(div);
      });
    }

    function kirimPesan() {
      const nama = document.getElementById('nama').value.trim();
      const pesan = document.getElementById('pesan').value.trim();
      const fotoInput = document.getElementById('foto');

      if (!pesan) {
        alert('Pesan tidak boleh kosong!');
        return;
      }

      if (fotoInput.files.length > 0) {
        const reader = new FileReader();
        reader.onload = function(e) {
          simpanPesan(nama, pesan, e.target.result);
        };
        reader.readAsDataURL(fotoInput.files[0]);
      } else {
        simpanPesan(nama, pesan, null);
      }
    }

    function simpanPesan(nama, pesan, foto) {
      const pesanBaru = { nama, pesan, foto };
      dataPesan.unshift(pesanBaru);
      localStorage.setItem('pesanAnonim', JSON.stringify(dataPesan));
      tampilkanPesan();
      document.getElementById('nama').value = '';
      document.getElementById('pesan').value = '';
      document.getElementById('foto').value = '';
    }

    tampilkanPesan();
  </script>
</body>
</html>

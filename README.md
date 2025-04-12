# Stock-Makanan
Menampilkan daftar makanan &amp; stok

<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Stok Makanan Interaktif</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
    }
    form {
      display: flex;
      flex-direction: column;
      gap: 10px;
      max-width: 400px;
      margin: 0 auto 30px auto;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    }
    input, button {
      padding: 10px;
      font-size: 16px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      background: #fff;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    }
    th, td {
      padding: 12px;
      border-bottom: 1px solid #ccc;
      text-align: left;
    }
    .status.cukup { color: green; font-weight: bold; }
    .status.hampir { color: orange; font-weight: bold; }
    .status.habis { color: red; font-weight: bold; }
    .delete-btn {
      background: red;
      color: white;
      border: none;
      padding: 6px 10px;
      cursor: pointer;
      border-radius: 4px;
    }
  </style>
</head>
<body>
  <h1>📦 Pengingat Stok Makanan</h1>

  <form id="form">
    <input type="text" id="nama" placeholder="Nama Makanan" required>
    <input type="text" id="jumlah" placeholder="Jumlah (cth: 3 kg, 5 butir)" required>
    <button type="submit">Tambah</button>
  </form>

  <table id="tabel">
    <thead>
      <tr>
        <th>Nama Makanan</th>
        <th>Jumlah</th>
        <th>Status</th>
        <th>Aksi</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    const form = document.getElementById('form');
    const tabelBody = document.querySelector('#tabel tbody');
    const namaInput = document.getElementById('nama');
    const jumlahInput = document.getElementById('jumlah');

    let data = JSON.parse(localStorage.getItem('stokMakanan')) || [];

    function renderTable() {
      tabelBody.innerHTML = '';
      data.forEach((item, index) => {
        const tr = document.createElement('tr');
        const statusClass = item.jumlah.includes('0') ? 'habis' :
                            parseInt(item.jumlah) <= 2 ? 'hampir' : 'cukup';
        tr.innerHTML = `
          <td>${item.nama}</td>
          <td>${item.jumlah}</td>
          <td class="status ${statusClass}">${statusClass.charAt(0).toUpperCase() + statusClass.slice(1)}</td>
          <td><button class="delete-btn" onclick="hapus(${index})">Hapus</button></td>
        `;
        tabelBody.appendChild(tr);
      });
    }

    function hapus(index) {
      data.splice(index, 1);
      simpanData();
      renderTable();
    }

    function simpanData() {
      localStorage.setItem('stokMakanan', JSON.stringify(data));
    }

    form.addEventListener('submit', (e) => {
      e.preventDefault();
      const nama = namaInput.value.trim();
      const jumlah = jumlahInput.value.trim();
      if (nama && jumlah) {
        data.push({ nama, jumlah });
        simpanData();
        renderTable();
        namaInput.value = '';
        jumlahInput.value = '';
      }
    });

    renderTable();
  </script>
</body>
</html>

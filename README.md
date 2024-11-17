# AplikasiCekCuaca
 Tugas 6 - (Muhammad Fajar Fitrianto_2210010748)

 Kelas : 5B NonReg Banjarmasin

 # Read.Me untuk Aplikasi Cek Cuaca

## Gambaran Umum

Aplikasi **Cek Cuaca** adalah aplikasi berbasis Java Swing yang memungkinkan pengguna untuk memeriksa kondisi cuaca di berbagai kota. Aplikasi ini menggunakan API dari OpenWeatherMap untuk mendapatkan data cuaca secara real-time.

## Fitur

- **Pilih Kota**: Pengguna dapat memilih kota dari dropdown yang telah disediakan.
- **Cek Cuaca**: Setelah memilih kota, pengguna dapat mengeklik tombol "CARI" untuk mendapatkan informasi cuaca terkini, termasuk deskripsi cuaca dan suhu.
- **Tampilan Hasil**: Hasil cuaca ditampilkan di label yang dapat diperbarui secara dinamis.

## Persyaratan

- Java Development Kit (JDK) 8 atau lebih tinggi
- Koneksi internet untuk mengakses API OpenWeatherMap

## Memulai

1. **Clone Repositori**: Unduh atau clone repositori yang berisi kode sumber.

2. **Buka Proyek**: Buka proyek di IDE Java pilihan Anda.

3. **Kompilasi dan Jalankan**: Kompilasi proyek dan jalankan kelas `CekCuacaForm`.

## Struktur Kode

Kelas utama untuk aplikasi ini adalah `CekCuacaForm`. Berikut adalah komponen kunci dari kode:

- **API Key dan URL**: Aplikasi menggunakan API Key dari OpenWeatherMap dan URL dasar untuk mengakses data cuaca.
- **Metode `getWeatherData`**: Metode ini mengambil data cuaca dari API berdasarkan nama kota yang diberikan.
- **Metode `parseWeatherData`**: Metode ini mem-parsing data JSON yang diterima dari API untuk mendapatkan informasi cuaca yang diperlukan.
- **Event Listener**: Listener aksi ditambahkan ke tombol untuk menangani interaksi pengguna dan menampilkan hasil cuaca.

## Kode Sumber

Berikut adalah kode sumber untuk aplikasi Cek Cuaca:

```java
import org.json.JSONObject;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class CekCuacaForm extends javax.swing.JFrame {
   private static final String API_KEY = "71b61226d0ea20506afe1b76aae5ef27"; // Ganti dengan API Key Anda
   private static final String BASE_URL = "http://api.openweathermap.org/data/2.5/weather?q=";

    public static String getWeatherData(String kota) {
        try {
            String urlString = BASE_URL + kota + "&appid=" + API_KEY + "&units=metric";
            URL url = new URL(urlString);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");

            BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            String inputLine;
            StringBuilder response = new StringBuilder();

            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
            in.close();

            return response.toString();
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    public CekCuacaForm() {
        initComponents();
        btnCekCuaca.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String kota = txtKota.getSelectedItem().toString();
                String response = getWeatherData(kota);
                if (response != null) {
                    JSONObject json = parseWeatherData(response);
                    String cuaca = json.getJSONArray("weather").getJSONObject(0).getString("description");
                    double suhu = json.getJSONObject("main").getDouble("temp");

                    lblHasil.setText("<html>Cuaca: " + cuaca + "<br>Suhu: " + suhu + "°C</html>");
                } else {
                    lblHasil.setText("Data tidak ditemukan!");
                }
            }
        });
    }

    public static JSONObject parseWeatherData(String data) {
        return new JSONObject(data);
    }

    public static void main(String args[]) {
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new

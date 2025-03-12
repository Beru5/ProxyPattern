# Proxy Design Pattern

## Apa itu Proxy Design Pattern?
Proxy Design Pattern adalah salah satu pola desain dalam pemrograman berorientasi objek yang masuk dalam kategori structural design patterns. Fungsinya adalah menyediakan objek pengganti atau perantara untuk objek lain, sehingga memungkinkan untuk mengontrol akses atau menambahkan fungsionalitas tambahan sebelum atau setelah interaksi dengan objek asli dilakukan.

## Kapan Proxy Design Pattern Digunakan?
### - Ketika diperlukan Lazy Loading
Proxy Design Patterns digunakan saat objek memerlukan sumber daya besar (semisalnya, gambar atau data besar) dan kita ingin menunda pembuatannya sampai benar benar dibutuhkan.
### - Ketika diperlukan Firewall
Proxy Design Patterns digunakan ketika diperlukan untuk membatasi akses ke objek berdasarkan hak pengguna atau aturan tertentu.
### - Ketika diperlukan Cache
Proxy Pattern apabila ingin menyimpan objek yang sudah di eksekusi agar tidak perlu dieksekusi berulang kali dengan menyimpan cache objek tersebut.

## Apa Kelebihan dari Proxy Design Pattern?
### 1. ..
[Input Text Here]

## Apa Kekurangan dari Proxy Design Pattern?
### - Tambahan Kompleksitas (Added Complexity) 
Menggunakan Proxy menambah lapisan ekstra pada sistem, yang dapat membuat kode lebih kompleks dan sulit diikuti.
### - Performa yang Berkurang (Decreased Performance) 
Meskipun Proxy dapat meningkatkan kinerja dalam beberapa kasus, ia juga dapat menambah latensi tambahan karena adanya lapisan perantara.
### - Ketergantungan Tambahan (Additional Dependencies) 
Proxy menambah ketergantungan tambahan pada sistem, yang bisa mempengaruhi stabilitas dan fleksibilitas aplikasi secara keseluruhan.

## Bagaimana Contoh Kode Penggunaan Proxy Design Pattern?
Berikut contoh kode proxy yang meliputi firewall, lazy loading, dan cache proxy:
```java
import java.util.*;

interface Resource {
    void access(String resource);
}

class RealResource implements Resource {
    @Override
    public void access(String resource) {
        if (resource.endsWith(".jpg")) {
            System.out.println("Loading image from disk: " + resource);
        } else {
            System.out.println("Connecting to " + resource);
        }
    }
}

class ProxyResource implements Resource {
    private RealResource realResource;
    private static List<String> bannedSites = Arrays.asList("facebook.com", "youtube.com", "tiktok.com");
    private Map<String, String> cache = new HashMap<>(); 

    public ProxyResource() {
        this.realResource = new RealResource();
    }

    @Override
    public void access(String resource) {
        if (resource.endsWith(".jpg")) {
            if (cache.containsKey(resource)) {
                System.out.println("Fetching image from cache: " + resource);
            } else {
                realResource.access(resource);
                cache.put(resource, "Loaded"); 
            }
        } else {
            if (bannedSites.contains(resource.toLowerCase())) {
                System.out.println("Access Denied to " + resource);
            } else {
                realResource.access(resource);
            }
        }
    }
}

public class ProxyPatternAdvancedDemo {
    public static void main(String[] args) {
        Resource proxy = new ProxyResource();

        proxy.access("google.com");   
        proxy.access("facebook.com");  
        System.out.println("-----------------------------------");
        proxy.access("photo1.jpg"); 
        proxy.access("photo2.jpg");
        proxy.access("photo1.jpg"); 
    }
}
```
Dari kode diatas, didapatkan output sebagai berikut:
```java
Connecting to google.com
Access Denied to facebook.com
-----------------------------------
Loading image from disk: photo1.jpg
Loading image from disk: photo2.jpg
Fetching image from cache: photo1.jpg
```
Dari output diatas, client akan terhubung ke google.com namun aksesnya ke facebook.com ditolak. Hal ini dapat terjadi dikarenakan class ProxyResource terdapat kode ```private static List<String> bannedSites = Arrays.asList("facebook.com", "youtube.com", "tiktok.com");``` yang merupakan firewall dan akan membatasi akses client ke web tertentu yang terbanned yang pada contoh kode ini berupa facebook.com, youtube.com, dan tiktok.com sementara google.com tidak sehingga client dapat dengan bebas mengakses web tertentu.
<br>
Kemudian client juga akan melakukan loading pada image dari 

package UTS2;

import java.util.Scanner;
// andreas

public class sdl {

    static class Node {

        String productName;
        int price;
        int quantity;
        Node next;

        Node(String productName, int price, int quantity) {
            this.productName = productName;
            this.price = price;
            this.quantity = quantity;
            this.next = null;
        }
    }

    static Scanner scanner = new Scanner(System.in);
    static Node shoppingCart = null;

    static String[] makanan = {"Mie instan 80gr", "Bubur instant 46gr", "Sereal 300gr", "Sarden 155gr", "Pasta 230gr"};
    static int[] hargaMakanan = {3000, 5000, 40000, 10000, 23000};
    static int[] stokMakanan = {100, 100, 100, 100, 100};

    static String[] minuman = {"Air Mineral 600ml", "Cola 500ml", "Jus 500ml", "Susu 500ml", "Kopi 350ml"};
    static int[] hargaMinuman = {3500, 5000, 7000, 8000, 4500};
    static int[] stokMinuman = {100, 100, 100, 100, 100};

    static String[] kebersihan = {"Sabun 75g", "Shampo 160ml", "Pasta gigi 160ml", "Deterjen 800gr", "Pengharum ruang 225ml"};
    static int[] hargaKebersihan = {3000, 19500, 20000, 25000, 34500};
    static int[] stokKebersihan = {100, 100, 100, 100, 100};

    static String[] buah = {"Pisang Kepok 1 sisir", "Apel 1 kg", "Naga", "Melon 1/2 buah", "Jeruk 1/2 kg"};
    static int[] hargaBuah = {33000, 12000, 14000, 10000, 16000};
    static int[] stokBuah = {100, 100, 100, 100, 100};

    static String[] kesehatan = {"Vitamin 5pcs", "Obat batuk 100ml", "Obat sakit kepala 4pcs", "Obat turun panas 10pcs", "Obat maag 8pcs"};
    static int[] hargaKesehatan = {7000, 8500, 3000, 2500, 1000};
    static int[] stokKesehatan = {100, 100, 100, 100, 100};

    public static void main(String[] args) {
        System.out.println("Selamat datang di MiniMart");
        menuUtama();
        System.out.println("Terima kasih telah berkunjung ke MiniMart");
        scanner.close();
    }

    static void menuUtama() {
        int pilihan;
        do {
            System.out.println("\nKategori Barang:");
            System.out.println("1. Makanan\n2. Minuman\n3. Produk Kebersihan\n4. Buah-buahan\n5. Produk Kesehatan");
            System.out.println("6. Hapus Barang dari Keranjang Belanja\n7. Cetak Keranjang Belanja\n8. Keluar");
            System.out.print("Pilihan: ");
            pilihan = scanner.nextInt();
            scanner.nextLine(); // membuat line kosong
            switch (pilihan) {
                case 1 ->
                    menuKategori(makanan, hargaMakanan, stokMakanan, "Makanan");
                case 2 ->
                    menuKategori(minuman, hargaMinuman, stokMinuman, "Minuman");
                case 3 ->
                    menuKategori(kebersihan, hargaKebersihan, stokKebersihan, "Produk Kebersihan");
                case 4 ->
                    menuKategori(buah, hargaBuah, stokBuah, "Buah-buahan");
                case 5 ->
                    menuKategori(kesehatan, hargaKesehatan, stokKesehatan, "Produk Kesehatan");
                case 6 ->
                    hapusBelanja();
                case 7 ->
                    cetakKeranjangBelanja();
                case 8 -> {
                    int totalHarga = cetakKeranjangBelanja();
                    System.out.println("Total Harga Semua Barang: " + totalHarga);
                }
                default ->
                    System.out.println("\nPilihan tidak tersedia\n");
            }
        } while (pilihan != 8);
    }

    //floren
    static void menuKategori(String[] products, int[] prices, int[] stock, String categoryName) {

        System.out.println("\nKategori: " + categoryName);

        System.out.println("No Produk\t\t\tHarga\t\tTersedia");
        for (int i = 0; i < products.length; i++) {
            System.out.printf("%d. %-23s\t%,d\t\t%d Pcs\n", (i + 1), products[i], prices[i], stock[i]);
        }

        System.out.print("Pilih nomor produk yang ingin dibeli (0 untuk kembali): ");
        int selectedProduct = scanner.nextInt();

        if (selectedProduct >= 1 && selectedProduct <= products.length) {
            tambahProduk(products, prices, stock, selectedProduct - 1);
        } else if (selectedProduct != 0) {

            System.out.println("\nPilihan tidak valid\n");
        }
    }

    static void tambahProduk(String[] products, int[] prices, int[] stock, int productIndex) {
        System.out.print("Masukkan jumlah barang yang ingin dibeli: ");
        int quantity = scanner.nextInt();
        if (quantity <= stock[productIndex]) {
            shoppingCart = tambahBelanja(products[productIndex], prices[productIndex], quantity, shoppingCart);
            stock[productIndex] -= quantity;
            System.out.println(quantity + " " + products[productIndex] + " telah ditambahkan ke dalam keranjang belanja.");
        } else {

            System.out.println("Stok produk tidak mencukupi.");
        }
    }

    static Node tambahBelanja(String productName, int price, int quantity, Node head) {
        Node newNode = new Node(productName, price, quantity);

        if (head == null) {
            return newNode;
        }
        Node current = head;
        while (current.next != null) {
            current = current.next;
        }
        current.next = newNode;
        return head;
    }

    static int cetakKeranjangBelanja() {
        if (shoppingCart == null) {
            System.out.println("\nKeranjang belanja kosong.");
            return 0;
        }
        System.out.println("\nDaftar Belanja:");
        System.out.println("------------------------------------------------------");
        System.out.println("No \tNama Produk     \tHarga  \tJumlah \tSubtotal");
        System.out.println("------------------------------------------------------");
        Node current = shoppingCart;
        int totalHarga = 0;
        int index = 1;

        while (current != null) {
            System.out.printf("%d\t%-20s\t%,d\t%d\t%,d\n", index, current.productName, current.price, current.quantity, current.price * current.quantity);
            totalHarga += current.price * current.quantity;
            current = current.next;
            index++;
        }
        return totalHarga;
    }

    //lia
    static void hapusBelanja() {
        if (shoppingCart == null) {
            System.out.println("\nKeranjang belanja kosong.");
            return;
        }

        System.out.println("\nDaftar Belanja:");
        Node current = shoppingCart;
        int index = 1;
        while (current != null) {
            System.out.println(index + ". " + current.productName + " - Jumlah: " + current.quantity);
            current = current.next;
            index++;
        }

        System.out.print("Pilih nomor produk yang ingin dikurangi jumlahnya (0 untuk kembali ke menu utama): ");
        int selectedProduct = scanner.nextInt();
        if (selectedProduct >= 1 && selectedProduct < index) {
            kurangiJumlahBelanja(selectedProduct);
        } else if (selectedProduct != 0) {
            System.out.println("\nPilihan tidak valid\n");
        }
    }

    static void kurangiJumlahBelanja(int index) {
        Node current = shoppingCart;
        int count = 1;

        // Cari node yang akan dikurangi jumlahnya
        while (current != null && count != index) {
            current = current.next;
            count++;
        }

        // Kurangi jumlah barang jika ditemukan
        if (current != null) {
            System.out.print("Masukkan jumlah barang yang ingin dikurangi: ");
            int quantityToRemove = scanner.nextInt();

            if (quantityToRemove <= current.quantity) {
                // Kurangi jumlah barang
                current.quantity -= quantityToRemove;

                // Tampilkan pesan
                System.out.println(quantityToRemove + " " + current.productName + " telah dikurangi dari daftar belanja.");
                if (current.quantity == 0) {
                    // Jika jumlah barang menjadi 0, hapus dari daftar belanja
                    shoppingCart = hapusProduk(index, shoppingCart);
                }
            } else {
                System.out.println("Jumlah yang ingin dikurangi melebihi jumlah yang ada dalam keranjang belanja.");
            }
        }
    }

    static Node hapusProduk(int index, Node head) {
        if (index == 1) {
            return head.next;
        }
        Node current = head;
        int count = 1;
        while (count < index - 1) {
            current = current.next;
            count++;
        }
        current.next = current.next.next;
        return head;
    }
}

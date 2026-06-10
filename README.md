#include <iostream>
#include <string>
using namespace std;

const int MAX = 100;

// ====================== STRUCT ======================
struct User {
    string username;
    string password;
};

struct Konser {
    int id;
    string nama;
    string lokasi;
    int harga;
    int stok;
};

struct Transaksi {
    string pembeli;
    int idKonser;
    int jumlah;
    int total;
};

// ====================== ARRAY ======================
Konser konser[MAX];
int jumlahKonser = 0;

User users[MAX];
int jumlahUser = 1;

// akun default
// admin / admin

// ====================== STACK ======================
struct StackNode {
    string pembeli;
    int idKonser;
    StackNode* next;
};

StackNode* topStack = NULL;

// ====================== QUEUE ======================
struct QueueNode {
    string pembeli;
    int idKonser;
    QueueNode* next;
};

QueueNode* frontQ = NULL;
QueueNode* rearQ = NULL;

// ====================== LINKED LIST ======================
struct Riwayat {
    string pembeli;
    int idKonser;
    int total;
    Riwayat* next;
};

Riwayat* head = NULL;

// ====================== LOGIN ======================
void initUser() {
    users[0].username = "admin";
    users[0].password = "admin";
}

bool login(string &userAktif) {

    string user, pass;

    cout << "\nUsername : ";
    cin >> user;

    cout << "Password : ";
    cin >> pass;

    for(int i=0;i<jumlahUser;i++) {

        if(users[i].username == user &&
           users[i].password == pass) {

            userAktif = user;
            return true;
        }
    }

    return false;
}

void registrasi() {

    cout << "\nUsername Baru : ";
    cin >> users[jumlahUser].username;

    cout << "Password Baru : ";
    cin >> users[jumlahUser].password;

    jumlahUser++;

    cout << "\nRegistrasi berhasil!\n";
}

// ====================== CRUD ======================
void tambahKonser() {

    Konser *ptr = &konser[jumlahKonser];

    cout << "\nID : ";
    cin >> ptr->id;
    cin.ignore();

    cout << "Nama Konser : ";
    getline(cin, ptr->nama);

    cout << "Lokasi : ";
    getline(cin, ptr->lokasi);

    cout << "Harga : ";
    cin >> ptr->harga;

    cout << "Stok : ";
    cin >> ptr->stok;

    jumlahKonser++;

    cout << "\nData berhasil ditambah.\n";
}

void tampilKonser() {

    cout << "\n===== DATA KONSER =====\n";

    for(int i=0;i<jumlahKonser;i++) {

        cout << "\nID     : " << konser[i].id;
        cout << "\nNama   : " << konser[i].nama;
        cout << "\nLokasi : " << konser[i].lokasi;
        cout << "\nHarga  : " << konser[i].harga;
        cout << "\nStok   : " << konser[i].stok;
        cout << "\n----------------------";
    }
}

void updateKonser() {

    int id;

    cout << "\nMasukkan ID : ";
    cin >> id;

    for(int i=0;i<jumlahKonser;i++) {

        if(konser[i].id == id) {

            cin.ignore();

            cout << "Nama Baru : ";
            getline(cin, konser[i].nama);

            cout << "Lokasi Baru : ";
            getline(cin, konser[i].lokasi);

            cout << "Harga Baru : ";
            cin >> konser[i].harga;

            cout << "Stok Baru : ";
            cin >> konser[i].stok;

            cout << "\nUpdate berhasil.\n";
            return;
        }
    }

    cout << "\nData tidak ditemukan.\n";
}

void hapusKonser() {

    int id;

    cout << "\nID yang dihapus : ";
    cin >> id;

    for(int i=0;i<jumlahKonser;i++) {

        if(konser[i].id == id) {

            for(int j=i;j<jumlahKonser-1;j++) {
                konser[j] = konser[j+1];
            }

            jumlahKonser--;

            cout << "\nData berhasil dihapus.\n";
            return;
        }
    }

    cout << "\nData tidak ditemukan.\n";
}

// ====================== SEARCH ======================
void cariKonser() {

    int id;

    cout << "\nCari ID : ";
    cin >> id;

    for(int i=0;i<jumlahKonser;i++) {

        if(konser[i].id == id) {

            cout << "\nDitemukan\n";
            cout << konser[i].nama << endl;
            return;
        }
    }

    cout << "\nTidak ditemukan.\n";
}

// ====================== SORT ======================
void bubbleSortHarga() {

    for(int i=0;i<jumlahKonser-1;i++) {

        for(int j=0;j<jumlahKonser-i-1;j++) {

            if(konser[j].harga > konser[j+1].harga) {

                Konser temp = konser[j];
                konser[j] = konser[j+1];
                konser[j+1] = temp;
            }
        }
    }

    cout << "\nData berhasil diurutkan.\n";
}

// ====================== STACK ======================
void pushUndo(string nama, int id) {

    StackNode* baru = new StackNode;

    baru->pembeli = nama;
    baru->idKonser = id;

    baru->next = topStack;
    topStack = baru;
}

void undoPesanan() {

    if(topStack == NULL) {

        cout << "\nTidak ada data.\n";
        return;
    }

    cout << "\nUndo : "
         << topStack->pembeli << endl;

    StackNode* hapus = topStack;

    topStack = topStack->next;

    delete hapus;
}

// ====================== QUEUE ======================
void enqueue(string nama, int id) {

    QueueNode* baru = new QueueNode;

    baru->pembeli = nama;
    baru->idKonser = id;
    baru->next = NULL;

    if(frontQ == NULL) {

        frontQ = rearQ = baru;
    }
    else {

        rearQ->next = baru;
        rearQ = baru;
    }
}

void tampilAntrian() {

    QueueNode* bantu = frontQ;

    cout << "\n===== ANTRIAN =====\n";

    while(bantu != NULL) {

        cout << bantu->pembeli << endl;

        bantu = bantu->next;
    }
}

// ====================== LINKED LIST ======================
void tambahRiwayat(string nama, int id, int total) {

    Riwayat* baru = new Riwayat;

    baru->pembeli = nama;
    baru->idKonser = id;
    baru->total = total;
    baru->next = NULL;

    if(head == NULL)
        head = baru;
    else {

        Riwayat* bantu = head;

        while(bantu->next != NULL)
            bantu = bantu->next;

        bantu->next = baru;
    }
}

void tampilRiwayat() {

    Riwayat* bantu = head;

    cout << "\n===== RIWAYAT =====\n";

    while(bantu != NULL) {

        cout << "Nama : "
             << bantu->pembeli
             << " | ID : "
             << bantu->idKonser
             << " | Total : "
             << bantu->total
             << endl;

        bantu = bantu->next;
    }
}

// ====================== STATISTIK ======================
int totalPendapatan = 0;
int totalTiket = 0;

void statistik() {

    cout << "\n===== STATISTIK =====\n";
    cout << "Total Tiket Terjual : "
         << totalTiket << endl;

    cout << "Pendapatan : Rp"
         << totalPendapatan << endl;
}

// ====================== PESAN ======================
void pesanTiket(string userAktif) {

    int id;
    int jumlah;

    tampilKonser();

    cout << "\nID Konser : ";
    cin >> id;

    cout << "Jumlah Tiket : ";
    cin >> jumlah;

    for(int i=0;i<jumlahKonser;i++) {

        if(konser[i].id == id) {

            if(konser[i].stok >= jumlah) {

                int total =
                    konser[i].harga * jumlah;

                konser[i].stok -= jumlah;

                totalPendapatan += total;
                totalTiket += jumlah;

                enqueue(userAktif,id);
                pushUndo(userAktif,id);
                tambahRiwayat(userAktif,id,total);

                cout << "\n===== NOTA =====\n";
                cout << "Nama : "
                     << userAktif << endl;

                cout << "Konser : "
                     << konser[i].nama << endl;

                cout << "Total : Rp"
                     << total << endl;

                return;
            }
        }
    }

    cout << "\nPesanan gagal.\n";
}

// ====================== MENU ======================
void menuAdmin() {

    int pilih;

    do {

        cout << "\n===== MENU ADMIN =====";
        cout << "\n1.Tambah";
        cout << "\n2.Tampil";
        cout << "\n3.Update";
        cout << "\n4.Hapus";
        cout << "\n5.Cari";
        cout << "\n6.Sort";
        cout << "\n7.Antrian";
        cout << "\n8.Riwayat";
        cout << "\n9.Statistik";
        cout << "\n0.Logout";
        cout << "\nPilih : ";
        cin >> pilih;

        switch(pilih) {

            case 1: tambahKonser(); break;
            case 2: tampilKonser(); break;
            case 3: updateKonser(); break;
            case 4: hapusKonser(); break;
            case 5: cariKonser(); break;
            case 6: bubbleSortHarga(); break;
            case 7: tampilAntrian(); break;
            case 8: tampilRiwayat(); break;
            case 9: statistik(); break;
        }

    } while(pilih != 0);
}

void menuUser(string userAktif) {

    int pilih;

    do {

        cout << "\n===== MENU USER =====";
        cout << "\n1.Lihat Konser";
        cout << "\n2.Pesan Tiket";
        cout << "\n3.Undo";
        cout << "\n4.Riwayat";
        cout << "\n0.Logout";
        cout << "\nPilih : ";
        cin >> pilih;

        switch(pilih) {

            case 1: tampilKonser(); break;
            case 2: pesanTiket(userAktif); break;
            case 3: undoPesanan(); break;
            case 4: tampilRiwayat(); break;
        }

    } while(pilih != 0);
}

// ====================== MAIN ======================
int main() {

    initUser();

    int pilih;
    string userAktif;

    do {

        cout << "\n==============================";
        cout << "\n SISTEM TIKET KONSER";
        cout << "\n==============================";
        cout << "\n1.Login";
        cout << "\n2.Registrasi";
        cout << "\n0.Keluar";
        cout << "\nPilih : ";
        cin >> pilih;

        switch(pilih) {

            case 1:

                if(login(userAktif)) {

                    if(userAktif == "admin")
                        menuAdmin();
                    else
                        menuUser(userAktif);
                }
                else {

                    cout << "\nLogin gagal.\n";
                }

                break;

            case 2:
                registrasi();
                break;
        }

    } while(pilih != 0);

    return 0;
}

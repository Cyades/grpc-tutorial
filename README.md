# AdvProg Modul 8
**Nama:** Malvin Scafi  
**NPM:** 2306152430  
**Kelas:** Pemrograman Lanjut A

---
### ✨ Reflection

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

    **Unary RPC**: Satu permintaan, satu balasan. Cocok untuk operasi CRUD sederhana seperti autentikasi atau `ProcessPayment`.
    
    **Server Streaming RPC**: Satu permintaan, banyak balasan. Ideal untuk data besar seperti pencarian, download file, atau `GetTransactionHistory`.
    
    **Bi-directional Streaming**: Aliran pesan dua arah secara independen. Sempurna untuk komunikasi real-time seperti aplikasi chat atau game online (`Chat` service).

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

    **Autentikasi**: JWT/OAuth2, interceptor untuk validasi token.
    
    **Otorisasi**: RBAC, validasi izin per-service/method.
    
    **Enkripsi**: TLS/SSL, sertifikat untuk saluran gRPC, enkripsi end-to-end.
    
    **Tambahan**: Rate limiting, validasi input ketat, logging untuk audit, penanganan error aman.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

    **Konkurensi**: Penanganan multiple streams, race condition, thread safety dengan ownership Rust.
    
    **Error**: Deteksi pemutusan koneksi, recovery dari kegagalan jaringan.
    
    **Resource**: Mencegah memory leak, pembatalan stream yang tepat, pengelolaan backpressure.
    
    **State**: Sinkronisasi state antar streams, penyimpanan state saat restart, koordinasi broadcast.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

    **Kelebihan**: Integrasi mulus dengan Tokio, antarmuka asinkron jelas, backpressure otomatis, pembatalan bersih.
    
    **Kekurangan**: Kompleksitas debugging, potensi deadlock, overhead performa, kurva pembelajaran Tokio yang curam.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

    **Pemisahan Concern**: Pisahkan definisi service dari implementasi, abstraksi logika bisnis.
    
    **Trait-Based Design**: Implementasi trait untuk perilaku service, trait bounds untuk fungsi generik.
    
    **Modularisasi**: Organisasi modul berdasarkan domain, crate terpisah untuk komponen reusable.
    
    **Error Handling**: Error types terstruktur, konversi ke Status gRPC.
    
    **Konfigurasi**: Eksternalisasi config, feature flags untuk komponen opsional.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

    **Validasi**: Input yang ketat, deteksi fraud, enkripsi data sensitif.
    
    **Integrasi**: Koneksi ke payment gateway, database transaksi, layanan notifikasi.
    
    **Audit**: Logging komprehensif, manajemen transaksi, kemampuan rollback.
    
    **Error & Retry**: Strategi retry dengan backoff, circuit breaker, penanganan error aman.
    
    **Skalabilitas**: Caching, arsitektur asinkron, connection pooling.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

    **Contract-First**: Pendekatan "schema first", kontrak API jelas, code generation multi-bahasa.
    
    **Microservices**: Komunikasi efisien antar layanan, overhead rendah, streaming data.
    
    **Interoperabilitas**: Dukungan multi-bahasa, gRPC gateway untuk REST, tantangan HTTP/2.
    
    **Evolusi API**: Versioning terstruktur, backward compatibility, manajemen dependensi jelas.
    
    **Performa**: Komunikasi efisien, multiplexing koneksi, tantangan dengan load balancing tradisional.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

    | Aspek | HTTP/2 (gRPC) | HTTP/1.1 | WebSocket |
    |-------|---------------|----------|-----------|
    | Komunikasi | Multiplexing koneksi | Request-response sekuensial | Full-duplex |
    | Format | Binary protocol efisien | Text-based | Text/binary |
    | Header | Header compression | Header berulang | Overhead minimal setelah handshake |
    | Fitur | Server push | Tidak ada | Tidak ada |
    | Debugging | Kompleks (binary) | Mudah (plaintext) | Moderat |
    | Tooling | Terbatas | Sangat lengkap | Moderat |
    | Browser Support | Terbatas | Universal | Luas |
    | Type Safety | Ketat (dengan Protobuf) | Tidak ada | Tidak ada |

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

    | Aspek | REST API | gRPC Bi-directional Streaming |
    |-------|----------|-------------------------------|
    | Pola Komunikasi | Sinkron dan stateless | Asinkron dan stateful |
    | Koneksi | Baru per request (overhead handshake) | Koneksi persisten |
    | Real-time | Memerlukan polling atau WebSocket | Native streaming |
    | Backpressure | Tidak ada standar | Fitur bawaan HTTP/2 |
    | Use case | CRUD sederhana | Komunikasi berkelanjutan real-time |
    | Overhead | Header berulang, JSON parsing | Header compression, serialisasi efisien |
    | Skalabilitas | Terbatas untuk real-time | Tinggi untuk komunikasi dua arah |

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

    | Aspek | Protocol Buffers (gRPC) | JSON (REST API) |
    |-------|--------------------------|-----------------|
    | Type Safety | Validasi compile-time | Validasi runtime |
    | Evolusi API | Aturan kompatibilitas jelas | Fleksibel tapi rawan breaking changes |
    | Ukuran Payload | 30-70% lebih kecil | Human-readable tapi lebih besar |
    | Format | Binary | Text-based |
    | Dokumentasi | Skema sebagai dokumentasi | Memerlukan OpenAPI/Swagger |
    | Developer Experience | Kurva belajar tinggi | Familiar, mudah debug |
    | Fleksibilitas | Kaku, perlu kompilasi ulang | Sangat fleksibel, mudah dimodifikasi |
    | Kecepatan Parsing | Lebih cepat | Lebih lambat |
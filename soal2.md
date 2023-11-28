# Analisis Kode JavaScript

## Masalah pada Kode

Kode berikut memiliki potensi untuk menyebabkan loop tak terbatas (infinite loop). Mari kita breakdown permasalahannya:

```jsx
export function App() {
  const [numberOne, setNumberOne] = useState(0);
  const [numberTwo, setNumberTwo] = useState(0);

  useEffect(() => {
    setNumberTwo((pre) => pre + 1);
  }, [numberOne]);

  useEffect(() => {
    setNumberOne((pre) => pre + 1);
  }, [numberTwo]);

  return (
    <div>
      <div>One: {numberOne}</div>
      <div>Two: {numberTwo}</div>
    </div>
  );
}
```


### Penjelasan Kesalahan:

1. **Ketergantungan Silang Infinite Loop**:
   - Ketika komponen pertama kali dimount, useEffect pertama akan dipanggil karena ada perubahan pada `numberOne`. Ketika `setNumberTwo` dipanggil, itu akan menyebabkan perubahan pada `numberTwo`, sehingga useEffect kedua juga akan dipanggil.
   - Setelah itu, useEffect kedua dipanggil karena ada perubahan pada `numberTwo`, dan ini menyebabkan perubahan pada `numberOne`, sehingga useEffect pertama akan dipanggil lagi. Proses ini terus berulang, menciptakan loop tak terbatas antara useEffect pertama dan kedua.

### Cara Memperbaiki:

Untuk memperbaiki masalah ini, Anda dapat menambahkan kondisi agar useEffect tidak terus-menerus memanggil satu sama lain. Misalnya, Kita dapat menggunakan kondisi atau melakukan perubahan yang lebih spesifik pada state agar loop tidak terjadi. Berikut adalah contoh cara memperbaikinya:

```jsx
export function App() {
  const [numberOne, setNumberOne] = useState(0);
  const [numberTwo, setNumberTwo] = useState(0);

  useEffect(() => {
    if (numberOne < 5) {
      setNumberTwo((pre) => pre + 1);
    }
  }, [numberOne]);

  useEffect(() => {
    if (numberTwo < 5) {
      setNumberOne((pre) => pre + 1);
    }
  }, [numberTwo]);

  return (
    <div>
      <div>One: {numberOne}</div>
      <div>Two: {numberTwo}</div>
    </div>
  );
}
```



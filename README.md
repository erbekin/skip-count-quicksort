# Skip-Count QuickSort (SC-QuickSort)

Skip-Count QuickSort is a novel variant of 3-Way QuickSort, designed for 
datasets with many duplicate elements or already sorted arrays.  
It improves stability and performance using a **stateful skip counter** 
to manage pivot-equal elements.

---

## 🚀 Key Features
- Optimized for **duplicate-heavy datasets**
- Faster on **already sorted arrays**
- Uses **random pivot selection** to reduce worst-case scenarios
- Stateful design inspired by the **Dutch National Flag problem**

---

## 🧩 Algorithm Idea

1. Pick a random pivot in [low, high].
2. Maintain:
   - `i`: left pointer
   - `j`: right pointer
   - `s`: skip counter (pivot equals)
3. Partition loop:
   - Move `j` left while `a[j] > pivot`
   - If `a[i] < pivot`: shift left if needed, then `i++`
   - If `a[i] == pivot`: `s++`, `i++`
   - If `a[i] > pivot`: swap `a[i]` and `a[j]`
4. Adjust `i` with skip counter.
5. Recurse on [low, i) and (j, high].

---

## 📊 Benchmark Results

### 5 Million Integers

| Data Type    | 3Skip | 3-Way Classic | 2-Way Classic |
|--------------|-------|---------------|---------------|
| Random       | 2.20s | 2.11s         | 1.62s         |
| Sorted       | 0.26s | 0.82s         | 1.34s         |

> **Result:** SC-QuickSort shines on sorted and duplicate-heavy arrays.

---

## 📌 Java Implementation

```java
public static <T extends Comparable<T>> void quickSort3Skip(T[] a, int low, int high) {
    if (low >= high) return;

    int i = low;
    int j = high;
    int s = 0;
    T pivot = a[random.nextInt(high - low + 1) + low];

    while (i <= j) {
        while (a[j].compareTo(pivot) > 0)
            j--;

        if (i > j) break;

        int cmp = a[i].compareTo(pivot);
        if (cmp < 0) {
            while (s > 0) {
                swap(a, i, --i);
                s--;
            }
            i++;
        } else if (cmp == 0) {
            s++;
            i++;
        } else {
            swap(a, i, j);
        }
    }

    i -= s;
    quickSort3Skip(a, low, i - 1);
    quickSort3Skip(a, j + 1, high);
}

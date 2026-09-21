# Technical Interview Cheat Sheet

Cheat sheet singkat buat dihafal sebelum interview. Print atau buka di tab terpisah.

---

## JavaScript / TypeScript (Wajib Hafal)

### Var, Let, Const
- `var` → function-scoped, hoisted, bisa re-declare
- `let` → block-scoped, TDZ, bisa reassign
- `const` → block-scoped, TDZ, **tidak** bisa reassign (tapi object isinya bisa berubah)

### This Keyword
```js
// 1. Method call: this = objek pemilik
obj.method() // this = obj

// 2. Standalone: this = global (strict mode = undefined)
function fn() { return this; }

// 3. Arrow function: this = lexically inherited
const arrow = () => { return this; } // ambil dari luar

// 4. new keyword: this = objek baru
function Person(name) { this.name = name; }

// 5. call/apply/bind: this = argumen pertama
fn.call(obj);
```

### Promise Lifecycle
```
pending → fulfilled (resolve) → .then()
       → rejected (reject)    → .catch()
       → (selalu)             → .finally()
```

### Async/Await Rules
- `async` function **selalu** return Promise
- `await` hanya valid di dalam `async` function
- Error handling: `try/catch` (bukan `.catch()` chain)
- Parallel await: `Promise.all([a(), b(), c()])`
- Race condition: `Promise.race([a(), b()])`

### Closure (Interview Favorite)
```js
function createCounter() {
  let count = 0;          // private variable
  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count,
  };
}
const c = createCounter();
c.increment(); // 1
c.increment(); // 2
c.getCount();  // 2
// count tidak bisa diakses dari luar — encapsulation!
```

### Event Loop (Wajib Paham)
```
Call Stack → Web APIs → Task Queue (micro/macro) → Call Stack

Microtask queue (Promise.then, queueMicrotask) diproses SEBELUM macrotask queue (setTimeout, setInterval, I/O)
```
```js
console.log('1');           // sync
setTimeout(() => console.log('2'), 0);  // macrotask
Promise.resolve().then(() => console.log('3')); // microtask
console.log('4');           // sync
// Output: 1, 4, 3, 2
```

### Destructuring & Spread
```js
const [first, ...rest] = [1, 2, 3];   // first=1, rest=[2,3]
const { name, age } = person;          // property extraction
const copy = { ...original };          // shallow clone
const merged = { ...a, ...b };         // b overwrite a
const args = [...argumentsLike];       // array-like → array
```

### Map vs Object vs Set
| | Object | Map | Set |
|---|---|---|---|
| Key type | string/symbol only | any value | values only (unique) |
| Order | insertion order (ES6+) | insertion order | insertion order |
| Size | `Object.keys(obj).length` | `map.size` | `set.size` |
| Iteration | for...in, Object.entries() | `map.forEach()` | `set.forEach()` |
| Use case | fixed schema | dynamic key/freq lookup | deduplication |

---

## React / React Native (Wajib Hafal)

### Component Lifecycle (Class → Hooks Mapping)
| Class | Hook |
|-------|------|
| `constructor` | `useState` initial value |
| `componentDidMount` | `useEffect(() => {}, [])` |
| `componentDidUpdate` | `useEffect(() => {}, [dep])` |
| `componentWillUnmount` | `return () => {}` in useEffect |
| `shouldComponentUpdate` | `React.memo` |
| `getDerivedStateFromProps` | custom logic in render/useEffect |

### Core Hooks (Hafal Signature)
```jsx
// State
const [state, setState] = useState(initialValue);

// Side effects
useEffect(() => {
  // mount + update
  return () => { /* cleanup (unmount) */ };
}, [dependency1, dependency2]); // [] = mount only

// Memoized value
const memoValue = useMemo(() => compute(a, b), [a, b]);

// Memoized callback
const memoCallback = useCallback(() => doSomething(x), [x]);

// Ref (persist across renders without re-render)
const ref = useRef(initialValue);
ref.current = newValue; // doesn't trigger re-render!

// Context
const value = useContext(MyContext);

// Custom hook (COMPOSITION over inheritance)
function useCustomHook(dep) {
  const [val, setVal] = useState(null);
  useEffect(() => { /* fetch/subscribe */ }, [dep]);
  return val;
}
```

### Re-render Triggers (Wajib Tahu!)
✅ Trigger re-render:
- `setState` (new reference)
- `props` change (parent re-renders)
- `context` value change

❌ TIDAK trigger re-render:
- Mutating state directly (`arr.push()`, `obj.key = val`)
- `useRef.current = ...`
- Variable outside component

### Key React Rules
1. **Hooks only at top level** — jangan di dalam if/loop/nested function
2. **Only in React functions or custom hooks**
3. **Dependency array must be complete** — eslint `react-hooks/exhaustive-deps`
4. **Keys must be stable & unique** — jangan pakai index sebagai key kalau data bisa berubah urutannya

### Common Performance Pitfalls
```jsx
// ❌ BAD: inline function/object = new reference every render
<Button onClick={() => handleClick(id)} />
<List items={items.map(i => ({...i, extra: calc(i)}))} />

// ✅ GOOD: memoize
const onClick = useCallback(() => handleClick(id), [id]);
const memoItems = useMemo(() => items.map(i => ({...i, extra: calc(i)})), [items]);
```

### Expo Router Quick Reference
```
app/
├── _layout.tsx          → Root navigator (Stack)
├── index.tsx            → "/" (Home)
├── (tabs)/
│   ├── _layout.tsx      → Tab Navigator
│   ├── _layout.tsx      → Tab config (tabBarIcon, headerShown)
│   ├── index.tsx        → "/(tabs)" (Tab Home)
│   └── profile.tsx      → "/(tabs)/profile"
├── details/
│   └── [id].tsx         → "/details/:id" (Dynamic)
└── +not-found.tsx       → 404 page
```

Navigation:
```tsx
import { useRouter, useLocalSearchParams, Link } from 'expo-router';

// Programmatic
const router = useRouter();
router.push('/details/123');
router.back();
router.replace('/login');

// Get params
const { id } = useLocalSearchParams<{ id: string }>();

// Declarative
<Link href="/profile">Go to Profile</Link>
```

---

## Python / ML (Wajib Hafal)

### NumPy Essentials
```python
import numpy as np

# Create
a = np.array([1, 2, 3])           # shape (3,)
m = np.zeros((2, 3))               # shape (2, 3), float64
r = np.random.randn(3, 4)          # normal distribution
seq = np.arange(0, 10, 2)          # [0, 2, 4, 6, 8]

# Shape ops
a.reshape(2, 3)     # (6,) → (2, 3)
a.flatten()         # → 1D
a.T                 # transpose
a.squeeze()         # remove dim size 1
np.expand_dims(a, axis=0)  # add dim

# Indexing
a[1:3]              # slice
a[a > 2]            # boolean mask
a[[0, 2]]           # fancy indexing

# Broadcasting rules:
# 1. Align shapes from right
# 2. Dimensions with size 1 are stretched
# 3. Incompatible → error
```

### Pandas Essentials
```python
import pandas as pd

# Read/write
df = pd.read_csv('data.csv')
df.to_csv('output.csv', index=False)

# Inspect
df.head(5)
df.info()        # types, non-null counts
df.describe()    # stats for numeric cols
df.shape         # (rows, cols)

# Select
df['col']                    # Series
df[['col1', 'col2']]         # DataFrame (multi-col)
df.loc[row_label]            # label-based
df.iloc[row_index]           # position-based
df[df['age'] > 25]          # boolean filter

# Transform
df['new_col'] = df['col'].apply(lambda x: x * 2)
df.groupby('category')['value'].mean()
df.dropna()                  # remove missing
df.fillna(0)                 # fill missing
df.merge(other, on='key')    # SQL JOIN
```

### TensorFlow / Keras Quick Ref
```python
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers, models

# Sequential model
model = keras.Sequential([
    layers.Conv2D(32, (3,3), activation='relu', input_shape=(128,128,1)),
    layers.MaxPooling2D((2,2)),
    layers.Conv2D(64, (3,3), activation='relu'),
    layers.MaxPooling2D((2,2)),
    layers.Flatten(),
    layers.Dense(128, activation='relu'),
    layers.Dropout(0.5),
    layers.Dense(4, activation='softmax')  # 4 classes
])

model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)

# Training
history = model.fit(
    X_train, y_train,
    epochs=30,
    batch_size=32,
    validation_split=0.2,
    callbacks=[
        keras.callbacks.EarlyStopping(patience=5, restore_best_weights=True),
        keras.callbacks.ReduceLROnPlateau(factor=0.5, patience=3),
    ]
)

# Evaluate
test_loss, test_acc = model.evaluate(X_test, y_test)
y_pred = model.predict(X_test)

# Save/Load
model.save('my_model.h5')
loaded = keras.models.load_model('my_model.h5')

# Custom model (Functional API — untuk hybrid arch)
inputs = keras.Input(shape=(128, 128, 1))
x = layers.Conv2D(32, 3, activation='relu')(inputs)
x = layers.MaxPooling2D(2)(x)
# ... more layers ...
outputs = layers.Dense(4, activation='softmax')(x)
model = keras.Model(inputs, outputs)
```

### Preprocessing Pipeline (MRI/CV)
```python
import cv2
import numpy as np
from PIL import Image

def preprocess_mri(image_path, target_size=(128, 128)):
    """Full pipeline: load → grayscale → CLAHE → resize → normalize"""
    # Load
    img = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)
    
    # CLAHE (Contrast Limited Adaptive HE)
    clahe = cv2.createCLAHE(clipLimit=2.0, tileGridSize=(8,8))
    img = clahe.apply(img)
    
    # Resize
    img = cv2.resize(img, target_size)
    
    # Normalize to [0, 1]
    img = img.astype(np.float32) / 255.0
    
    # Add channel & batch dims: (1, 128, 128, 1)
    img = np.expand_dims(img, axis=-1)
    img = np.expand_dims(img, axis=0)
    
    return img
```

### Evaluation Metrics (Manual)
```python
import numpy as np
from sklearn.metrics import confusion_matrix, classification_report

# Confusion matrix
cm = confusion_matrix(y_true, y_pred)
print(cm)

# Per-class metrics
report = classification_report(y_true, y_pred, target_names=class_names)
print(report)

# Manual F1
def f1_score(precision, recall):
    if precision + recall == 0:
        return 0.0
    return 2 * (precision * recall) / (precision + recall)

# Manual accuracy
accuracy = np.sum(y_true == y_pred) / len(y_true)
```

---

## Git (Wajib Bisa)

### Commands yang Sering Dipakai
```bash
# Daily workflow
git status                          # cek state
git add .                           # stage semua
git commit -m "type: description"   # commit
git push origin main                # push ke remote

# Branching
git branch -a                       # liat semua branch (local + remote)
git checkout -b feature/new-feature # bikin + pindah ke branch baru
git merge main                      # merge main ke current branch

# Undo mistakes
git reset --soft HEAD~1             # undo commit, keep changes staged
git reset --hard HEAD~1             # undo commit, DISCARD changes
git restore <file>                  # undo changes to file (uncommitted)
git revert <commit-hash>            # undo commit dengan commit baru (safe!)

# Stash (simpan sementara)
git stash push -m "WIP feature X"
git stash pop                        # kembalikan

# Remote
git remote -v                        # liat remote URLs
git remote add origin <url>          # tambah remote
git fetch origin                     # download tanpa merge
git pull origin main                 # fetch + merge

# Log
git log --oneline -10                # 10 commit terakhir, 1 baris
git diff                             # liat perubahan belum staged
git diff --staged                    # liat perubahan sudah staged
```

### Commit Message Convention
```
feat: tambah fitur login OAuth
fix: bug navigasi tab di Android
docs: update README deployment
refactor: pindah state management ke Zustand
test: tambah unit test untuk API client
chore: upgrade dependencies
style: format code sesuai prettier
perf: optimasi FlatList rendering
```

---

## SQL (Wajib Dasar)

```sql
-- SELECT dasar
SELECT name, email FROM users WHERE active = true ORDER BY created_at DESC LIMIT 10;

-- JOIN
SELECT u.name, o.total FROM users u
LEFT JOIN orders o ON u.id = o.user_id;

-- AGGREGATE
SELECT category, COUNT(*) as total, AVG(price) as avg_price
FROM products
GROUP BY category
HAVING COUNT(*) > 5;

-- INSERT / UPDATE / DELETE
INSERT INTO users (name, email) VALUES ('Riski', 'riski@email.com');
UPDATE users SET active = false WHERE last_login < '2025-01-01';
DELETE FROM sessions WHERE expires_at < NOW();

-- INDEX (penting buat performa)
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_id ON orders(user_id);
```

---

## HTTP / REST API (Wajib Paham)

| Method | Semantik | Idempoten? | Safe? |
|--------|----------|------------|-------|
| GET | Ambil data | ✅ | ✅ |
| POST | Buat resource baru | ❌ | ❌ |
| PUT | Update full resource | ✅ | ❌ |
| PATCH | Partial update | ❌ | ❌ |
| DELETE | Hapus resource | ✅ | ❌ |

### Status Code Wajib Hafal
- **200 OK** — success (GET, PUT, PATCH)
- **201 Created** — resource berhasil dibuat (POST)
- **204 No Content** — success tapi tidak ada body (DELETE)
- **400 Bad Request** — request tidak valid
- **401 Unauthorized** — tidak ada auth / token expired
- **403 Forbidden** — ada auth tapi tidak punya akses
- **404 Not Found** — resource tidak ditemukan
- **429 Too Many Requests** — rate limit exceeded
- **500 Internal Server Error** — kesalahan server
- **502 Bad Gateway** — server upstream error
- **503 Service Unavailable** — server sementara down

---

## Design Patterns (Junior Level)

### Singleton
Satu instance saja selama aplikasi jalan.
```ts
class Database {
  private static instance: Database;
  private constructor() {}
  static getInstance(): Database {
    if (!Database.instance) {
      Database.instance = new Database();
    }
    return Database.instance;
  }
}
```

### Observer / Event Emitter
Satu object notify banyak listener.
```ts
class EventEmitter {
  private listeners: Record<string, Function[]> = {};
  on(event: string, cb: Function) {
    (this.listeners[event] ??= []).push(cb);
  }
  emit(event: string, ...args: any[]) {
    (this.listeners[event] ?? []).forEach(cb => cb(...args));
  }
}
```

### Factory
Buat object tanpa specify class konkret.
```ts
interface Button { render(): string; }
class PrimaryButton implements Button { render() { return '<button class="primary">'; } }
class SecondaryButton implements Button { render() { return '<button class="secondary">'; } }
function createButton(type: 'primary' | 'secondary'): Button {
  return type === 'primary' ? new PrimaryButton() : new SecondaryButton();
}
```

---

## Big-O Cheatsheet (Wajib Hafal)

| Operation | Array | Linked List | Hash Map | BST (balanced) |
|-----------|-------|-------------|----------|----------------|
| Access by index | O(1) | O(n) | - | - |
| Search | O(n) | O(n) | O(1) avg | O(log n) |
| Insert at end | O(1)* | O(1) | O(1) | O(log n) |
| Insert at start | O(n) | O(1) | O(1) | O(log n) |
| Delete | O(n) | O(n) | O(1) | O(log n) |
| Sort | O(n log n) | O(n log n) | - | O(n) (traversal) |

*Amortized

### Sorting Algorithms
| Algorithm | Best | Average | Worst | Space | Stable? |
|-----------|------|---------|-------|-------|---------|
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ |

---

## Behavioral Interview Framework: STAR

Setiap jawaban cerita proyek harus pakai format ini:

| | Artinya | Contoh |
|---|---|---|
| **S**ituation | Konteks apa? | "Saat skripsi, model ML cuma jalan di Colab..." |
| **T**ask | Tugas apa? | "Harus deploy supaya bisa diakses publik..." |
| **A**ction | Lu ngapain? | "Saya bikin Flask API, deploy ke Railway..." |
| **R**esult | Hasilnya? | "Model bisa diakses siapa saja via browser..." |

**Tips:** Fokus 70% di **Action** dan **Result**. Situation & Task cukup 1-2 kalimat.

---

## Acronyms Wajib Tahu

| Akronim | Arti |
|---------|------|
| API | Application Programming Interface |
| CRUD | Create, Read, Update, Delete |
| DTO | Data Transfer Object |
| IDE | Integrated Development Environment |
| SDK | Software Development Kit |
| CI/CD | Continuous Integration / Deployment |
| DOM | Document Object Model |
| SPA | Single Page Application |
| PWA | Progressive Web App |
| SSR | Server-Side Rendering |
| CSR | Client-Side Rendering |
| JWT | JSON Web Token |
| ORM | Object-Relational Mapping |
| ACID | Atomicity, Consistency, Isolation, Durability |
| CAP | Consistency, Availability, Partition tolerance |
| REST | Representational State Transfer |
| GraphQL | Query Language for APIs (alternative to REST) |
| CDN | Content Delivery Network |
| DNS | Domain Name System |
| SSL/TLS | Secure Sockets Layer / Transport Layer Security |
| SSH | Secure Shell |
| VPN | Virtual Private Network |
| VM | Virtual Machine |
| Container | Lightweight isolated environment (Docker) |
| MLOps | Machine Learning Operations |
| CRISP-DM | Cross-Industry Standard Process for Data Mining |
| SSIM | Structural Similarity Index |
| MSE | Mean Squared Error |
| CLAHE | Contrast Limited Adaptive Histogram Equalization |
| CNN | Convolutional Neural Network |
| RNN | Recurrent Neural Network |
| LSTM | Long Short-Term Memory |
| GAN | Generative Adversarial Network |
| NLP | Natural Language Processing |
| OCR | Optical Character Recognition |

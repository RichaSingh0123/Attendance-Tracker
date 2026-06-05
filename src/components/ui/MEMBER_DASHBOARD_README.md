# Member Dashboard – Person 4

React component for the **Founders Club Attendance Tracker** covering all Person 4 responsibilities:
member dashboard UI, attendance stats, history table with filters, analytics charts, and profile page.

---

## Things to Change Before Pushing

### 1. User Data
Replace the `MOCK_USER` object at the top of `MemberDashboard.jsx` with a real Supabase fetch.

```js
// Current (mock) — lines 4–11
const MOCK_USER = {
  id: "user-001",           // ← replace with auth session user ID
  name: "Richa",            // ← replace with user's display name from DB
  email: "richa@foundersclub.org", // ← replace with user's email from auth
  role: "Member",           // ← replace with role from users table
  joinedAt: "2024-08-01",   // ← replace with created_at from users table
  avatar: null,             // ← replace with avatar_url if your DB stores one
};
```

**Supabase equivalent (ask Person 1 for exact table/column names):**
```js
const { data: { user } } = await supabase.auth.getUser();
const { data: profile } = await supabase
  .from('users')
  .select('*')
  .eq('id', user.id)
  .single();
```

---

### 2. Attendance Records
Replace `MOCK_ATTENDANCE` with a real Supabase query.

```js
// Current (mock) — lines 13–26
const MOCK_ATTENDANCE = [
  { id: 1, meeting: "Weekly Sync #1", date: "2025-01-06", status: "present", duration: 60 },
  // ...
];
```

Each record needs these fields:

| Field | Type | Description |
|---|---|---|
| `id` | number/string | Unique record ID |
| `meeting` | string | Meeting name |
| `date` | string `YYYY-MM-DD` | Date of meeting |
| `status` | `"present"` or `"absent"` | Attendance status |
| `duration` | number | Duration in minutes |

**Supabase equivalent:**
```js
const { data: attendance } = await supabase
  .from('attendance')
  .select('id, status, meetings(name, date, duration)')
  .eq('user_id', user.id)
  .order('date', { ascending: false });
```

---

### 3. Bar Chart Months
The bar chart currently hardcodes Jan–Mar. Update the `months` array to match your actual data range.

```js
// Line ~128
const months = ["Jan", "Feb", "Mar"]; // ← update to your active months
```

---

### 4. Default Theme
The dashboard opens in dark mode by default. Change to `"light"` if preferred.

```js
const [theme, setTheme] = useState("dark"); // ← change to "light" if needed
```

---

## How to Push to GitHub

### First-time setup (only once)

```bash
# 1. Fork the repo on GitHub (go to FC OS org → founders-attendance-frontend → Fork)

# 2. Clone your fork locally
git clone https://github.com/YOUR_USERNAME/founders-attendance-frontend.git
cd founders-attendance-frontend

# 3. Add the original repo as upstream (to pull future updates)
git remote add upstream https://github.com/FC-OS/founders-attendance-frontend.git
```

### Every time you push new work

```bash
# 1. Make sure you're on main and up to date
git checkout main
git pull upstream main

# 2. Create your feature branch (FeTrack = short for your role)
git checkout -b feature/FeTrack

# 3. Copy MemberDashboard.jsx into the right folder
#    (ask Person 2 for the exact src/ structure they set up)
cp MemberDashboard.jsx src/pages/MemberDashboard.jsx

# 4. Stage your files
git add src/pages/MemberDashboard.jsx

# 5. Commit with a clear message
git commit -m "Add member dashboard with attendance stats, history, charts, and profile"

# 6. Push to your fork
git push origin feature/FeTrack

# 7. Open GitHub → your fork → click "Compare & Pull Request"
#    Fill in the PR description explaining what changed and why
#    Wait for someone else to review and approve — don't merge your own PR
```

### Subsequent updates (e.g. swapping mock data for real Supabase calls)

```bash
git checkout feature/FeTrack       # go back to your branch
# make your changes...
git add src/pages/MemberDashboard.jsx
git commit -m "Connect member dashboard to Supabase attendance table"
git push origin feature/FeTrack    # updates the existing PR automatically
```

---

## File Location

```
founders-attendance-frontend/
└── src/
    └── pages/
        └── MemberDashboard.jsx   ← this file
```

## Route Setup (coordinate with Person 2)

```jsx
// In your router file (App.jsx or routes.jsx)
import MemberDashboard from "./pages/MemberDashboard";

<Route path="/dashboard" element={<MemberDashboard />} />
```

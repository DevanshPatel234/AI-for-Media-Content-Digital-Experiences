# 🧪 End-to-End Testing Guide

Complete walkthrough to test the full AIC² application locally.

## Prerequisites

Both servers must be running:
- ✅ Backend: http://localhost:5000
- ✅ Frontend: http://localhost:5173

## Test Flow: Register → Login → Dashboard → Logout

### Step 1: Access the Application

Open browser and go to:
```
http://localhost:5173
```

You should see the AIC² home page with:
- Title: "AIC²"
- Subtitle: "AI Content Intelligence Copilot"
- Features overview
- "Get Started" and "Login" buttons

✅ **Expected Result**: Home page loads without errors

---

### Step 2: Register a New Account

1. Click **"Get Started"** button
2. You'll be taken to `/register` page
3. Enter test credentials:
   - Email: `test@example.com`
   - Password: `password123`
   - Confirm Password: `password123`
4. Click **"Register"** button

✅ **Expected Result**: 
- Success message appears
- Automatically redirected to login page after 1.5 seconds
- No errors in browser console

❌ **If it fails**:
- Check backend is running
- Check email format is valid
- Verify password is at least 6 characters
- Look for error message from backend

---

### Step 3: Login

1. You should already be on login page `/login`
2. If not, click **"Login here"** link
3. Enter your credentials:
   - Email: `test@example.com`
   - Password: `password123`
4. Click **"Login"** button

✅ **Expected Result**:
- Success message: "Login successful! Redirecting to dashboard..."
- Token saved in localStorage
- Automatically redirected to dashboard after 1 second
- Can open DevTools → Application → LocalStorage and see `token` key

❌ **If it fails**:
- Verify credentials are correct
- Check backend is running
- Check network tab in DevTools for 401/403 errors
- Ensure JWT_SECRET is set in backend .env

---

### Step 4: Access Protected Dashboard

Once logged in, you should see the Dashboard at `/dashboard` with:

**Header Section**:
- Title: "AIC² Dashboard"
- Your email address displayed
- "Logout" button

**Generate Content Section**:
- Topic input field
- Platform dropdown (Twitter, LinkedIn, Instagram, Facebook, TikTok)
- Tone dropdown (Professional, Casual, Persuasive, Humorous, Educational)
- "Generate Content" button

**Analytics Section** (if available):
- Best Tone card
- Best Platform card
- Total Posts card
- Avg Engagement card

**Content History Section**:
- Shows "No content generated yet" if first time
- Each content card shows:
  - Topic
  - Preview text (first 100 characters)
  - Platform badge
  - Tone badge
  - Engagement count
  - "Simulate Engagement" button

✅ **Expected Result**: All sections load without errors

❌ **If you see login page instead**:
- Token might have expired
- LocalStorage might be cleared
- Refresh page and try again

---

### Step 5: Generate Content

1. In the "Generate Content" section:
   - Topic: `"The future of AI in marketing"`
   - Platform: `Twitter` (keep default)
   - Tone: `Professional` (keep default)

2. Click **"✨ Generate Content"** button

✅ **Expected Result**:
- Button shows "Generating..." while loading
- Success message appears: "Content generated successfully!"
- Content History updates with new post
- Post shows in the list with topic, tone badge, platform badge

❌ **If it fails**:
- Check OpenAI API key is valid in backend .env
- Check OpenAI account has credits
- Look for error message (usually OpenAI related)
- Check backend logs for detailed error

---

### Step 6: Simulate Engagement

In the generated content card:

1. Click **"Simulate Engagement"** button

✅ **Expected Result**:
- Button shows "Loading..." state
- Success message: "Engagement simulated! System learned from this post."
- Engagement count increases on the card
- Analytics section updates with new data
- Your preferences are updated based on performance

❌ **If it fails**:
- Check backend is running
- Verify content ID is valid
- Look at network tab in DevTools for error details

---

### Step 7: Check Analytics

Look at the Analytics section after simulating engagement:

**You should see:**
- Best Tone: Shows most successful tone (e.g., "professional")
- Best Platform: Shows best platform for you
- Total Posts: Number of posts generated (should be 1+)
- Avg Engagement: Average engagement score

**Trend Information**:
- Status: "Improving", "Stable", or "Declining"
- Change: Percentage change in recent posts

✅ **Expected Result**: Analytics update with data from your posts

❌ **If analytics are empty**:
- You need to simulate engagement on at least one post
- System needs 10 posts for trend analysis
- Generate and engage with a few more posts to see trends

---

### Step 8: Generate More Content

1. Create another post with different tone:
   - Topic: `"Best practices for remote work"`
   - Platform: `LinkedIn`
   - Tone: `Educational`

2. Click "Generate Content"

3. Simulate engagement on this new post

✅ **Expected Result**:
- Multiple content items in history
- Analytics update with new data
- System learns from different tones and platforms

---

### Step 9: Logout

1. Click **"Logout"** button in top-right

✅ **Expected Result**:
- Token cleared from localStorage
- Redirected to login page
- Cannot access dashboard without login

---

## 📊 Verification Checklist

- [x] Home page loads and displays correctly
- [x] Register page works with validation
- [x] Login page accepts credentials
- [x] JWT token saved in localStorage
- [x] Can access protected dashboard
- [x] Can generate content successfully
- [x] Engagement simulation works
- [x] Analytics update with new data
- [x] Logout clears token and redirects
- [x] Protected route blocks unauthenticated access
- [x] All error messages display properly
- [x] Responsive design works on different screen sizes

## 🔍 Testing Edge Cases

### Test 1: Network Error Handling

**Scenario**: Backend is down during login
```
1. Stop backend server (Ctrl+C)
2. Try to login
3. Should show error message
4. Restart backend
5. Should work again
```

✅ **Expected**: Error message appears, can retry after backend restarts

### Test 2: Token Expiration

**Scenario**: Manually delete token from localStorage
```
1. Open DevTools → Application → LocalStorage
2. Find "token" key and delete it
3. Try to interact with dashboard
4. Should redirect to login
```

✅ **Expected**: Protected routes redirect to login

### Test 3: Invalid Email Format

**Scenario**: Register with invalid email
```
1. Go to register page
2. Enter: `notanemail` (no @)
3. Click Register
```

✅ **Expected**: HTML5 validation prevents submission

### Test 4: Password Mismatch

**Scenario**: Register with mismatched passwords
```
1. Go to register page
2. Password: `password123`
3. Confirm: `password456`
4. Click Register
```

✅ **Expected**: Error: "Passwords do not match"

### Test 5: Duplicate Email

**Scenario**: Register with same email twice
```
1. Register with: test@example.com
2. Try to register again with: test@example.com
3. Click Register
```

✅ **Expected**: Error from backend: "Email already registered"

## 🐛 Debugging Tips

### Check Console for Errors
- Open DevTools: F12 or Right-click → Inspect
- Look at Console tab for JavaScript errors
- Look at Network tab to see API requests

### Check LocalStorage
- DevTools → Application → Local Storage
- Verify `token` and `email` are stored after login
- Verify they're cleared after logout

### Check Network Requests
- DevTools → Network tab
- Look for API requests to http://localhost:5000/api
- Check response status (200 = success, 401 = unauthorized)
- Look at response body for error details

### Check Backend Logs
- Look at terminal running backend
- See console.log output from Express server
- Check for MongoDB connection messages
- Look for error details

## 📋 Test Results Template

```
Date: _______________
Tester: _______________

✅ / ❌ Home page loads
✅ / ❌ Register works
✅ / ❌ Login works
✅ / ❌ Dashboard accessible
✅ / ❌ Content generation works
✅ / ❌ Engagement simulation works
✅ / ❌ Analytics display correctly
✅ / ❌ Logout works
✅ / ❌ Protected routes work

Issues found:
_____________________
_____________________

Overall Result: PASS / FAIL
```

## 🎯 Success Criteria

All tests pass when:
1. ✅ Registration works with validation
2. ✅ Login returns valid JWT token
3. ✅ Protected routes require authentication
4. ✅ Content generation works with OpenAI
5. ✅ Engagement simulation triggers learning
6. ✅ Analytics display correct data
7. ✅ Logout clears session
8. ✅ No errors in console
9. ✅ All API responses are 200/201
10. ✅ UI is responsive and user-friendly

If all criteria are met: 🎉 **System is ready for production!**

---

## ⚠️ Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| `Cannot GET /` | Frontend not running, start with `npm run dev` in client folder |
| `CORS error` | Backend might not have CORS enabled or is down |
| `401 Unauthorized` | JWT token invalid or expired, login again |
| `Cannot read property 'token'` | Login response doesn't include token, check backend |
| `OpenAI API error` | Check API key is valid and account has credits |
| `MongoDB connection failed` | Check MONGODB_URI and network access in Atlas |
| `White screen` | Check console for errors, reload page |

---

## 🚀 You Did It!

If you've completed all these steps successfully, you have a fully functional full-stack application! 🎉

Next steps:
- Deploy backend to production
- Deploy frontend to Vercel or similar
- Add real engagement tracking
- Build mobile app

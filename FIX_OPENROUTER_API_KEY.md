# Fix: OpenRouter API Key Error (401 - User not found)

## 🔴 Error
```
"Error generating embedding: API request failed with status 401: 
{"error":{"message":"User not found.","code":401}}"
```

## ✅ Solution

This error means your **OpenRouter API key is invalid or missing** in Render.

### Step 1: Get Your OpenRouter API Key

1. Go to [OpenRouter Dashboard](https://openrouter.ai/keys)
2. Sign in to your account
3. Click **"Create Key"** or copy an existing key
4. The key should look like: `sk-or-v1-xxxxxxxxxxxxx`

### Step 2: Add/Update Key in Render

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click your service: `demo-zlc2`
3. Click **"Environment"** tab (left sidebar)
4. Look for `OPENROUTER_API_KEY`:
   - **If it exists**: Click on it → Update the value → Paste your new key
   - **If it doesn't exist**: Click **"Add Environment Variable"**:
     - **Key**: `OPENROUTER_API_KEY`
     - **Value**: `sk-or-v1-xxxxxxxxxxxxx` (your actual key)
     - Click **"Save"**

### Step 3: Verify the Key Format

Your OpenRouter API key should:
- Start with `sk-or-v1-`
- Be about 50-60 characters long
- Have no spaces or quotes

**Wrong:**
```
OPENROUTER_API_KEY = "sk-or-v1-xxxxx"  ❌ (don't include quotes)
OPENROUTER_API_KEY = sk-or-v1-xxxxx    ✅ (correct)
```

### Step 4: Restart Your Service

After updating the key:
1. Render will automatically redeploy
2. Wait 1-2 minutes for deployment
3. Or manually click **"Manual Deploy"** → **"Deploy latest commit"**

### Step 5: Test Again

```bash
curl -X POST "https://demo-zlc2.onrender.com/api/chat" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "What is ROS2?",
    "conversation_id": "test-123"
  }'
```

---

## 🔍 Common Issues

### Issue 1: Key Not Set
**Symptom:** Same 401 error
**Fix:** Make sure `OPENROUTER_API_KEY` exists in Render Environment tab

### Issue 2: Invalid Key
**Symptom:** 401 "User not found"
**Fix:** 
- Generate a new key from OpenRouter
- Make sure you're copying the full key
- Check for extra spaces or characters

### Issue 3: Key Expired/Revoked
**Symptom:** 401 error
**Fix:** Create a new key in OpenRouter dashboard

### Issue 4: Wrong Account
**Symptom:** 401 error
**Fix:** Make sure you're using the key from the correct OpenRouter account

---

## 📋 Quick Checklist

- [ ] Logged into OpenRouter dashboard
- [ ] Created/copied API key (starts with `sk-or-v1-`)
- [ ] Added `OPENROUTER_API_KEY` in Render Environment tab
- [ ] Key has no quotes or spaces
- [ ] Service redeployed after adding key
- [ ] Tested chat endpoint again

---

## 💡 Pro Tips

1. **Never commit API keys to GitHub** - Always use environment variables
2. **Keep keys secure** - Don't share them publicly
3. **Test keys first** - You can test your key at: https://openrouter.ai/docs
4. **Check key permissions** - Make sure your key has access to the models you need

---

## 🚀 After Fixing

Once the key is set correctly:
1. The service will automatically redeploy
2. Wait 1-2 minutes
3. Test the chatbot again
4. It should work! 🎉

---

## 📞 Still Not Working?

If you still get errors after fixing the key:
1. Check Render logs for the exact error
2. Verify the key is active in OpenRouter dashboard
3. Make sure you have credits/balance in OpenRouter account
4. Check that your OpenRouter account has access to the embedding model


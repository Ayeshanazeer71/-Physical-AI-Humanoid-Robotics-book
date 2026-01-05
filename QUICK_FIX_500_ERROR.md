# Quick Fix: 500 Error in Chatbot

## 🚨 Immediate Steps to Fix

### Step 1: Check Render Logs (Most Important!)

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click your service: `demo-zlc2`
3. Click **"Logs"** tab
4. Look for the error when you try to chat
5. **Copy the error message** - this will tell us exactly what's wrong

### Step 2: Verify Environment Variables

In Render → Your Service → **Environment** tab, make sure these are set:

✅ **Required:**
- `OPENROUTER_API_KEY` - Your OpenRouter API key
- `QDRANT_URL` - Your Qdrant Cloud URL  
- `QDRANT_API_KEY` - Your Qdrant Cloud API key

**If any are missing:**
- Get OpenRouter key: https://openrouter.ai/keys
- Get Qdrant credentials: https://cloud.qdrant.io/

### Step 3: Test Health Endpoint

```bash
curl https://demo-zlc2.onrender.com/api/health
```

**Expected response:**
```json
{
  "status": "healthy",
  "qdrant_connected": true,
  "collection_exists": true
}
```

**If `collection_exists: false`:**
→ You need to ingest documents (see Step 4)

### Step 4: Ingest Documents (If Collection is Empty)

**Option A: Using Render Shell (Easiest)**

1. Render Dashboard → Your Service → **"Shell"** tab
2. Run:
```bash
cd backend
python reingest_docs.py
```

**Option B: Using API**

If you have access to your docs folder locally:
```bash
curl -X POST "https://demo-zlc2.onrender.com/api/ingest" \
  -H "Content-Type: application/json" \
  -d '{
    "source": "../docusaurus/docs",
    "recursive": true,
    "filters": [".md", ".mdx"]
  }'
```

---

## 🔍 Most Common Issues

### Issue 1: Missing Environment Variables (80% of cases)
**Symptom:** Error about missing API keys or connection failures
**Fix:** Add all 3 required variables in Render Environment tab

### Issue 2: Empty Qdrant Collection (15% of cases)
**Symptom:** `collection_exists: false` in health check
**Fix:** Ingest documents using Render Shell

### Issue 3: Invalid API Keys (5% of cases)
**Symptom:** Authentication errors in logs
**Fix:** Verify keys are correct and active

---

## 📋 Quick Checklist

- [ ] Check Render logs for specific error
- [ ] Verify `OPENROUTER_API_KEY` is set
- [ ] Verify `QDRANT_URL` is set
- [ ] Verify `QDRANT_API_KEY` is set
- [ ] Test `/api/health` endpoint
- [ ] Ingest documents if collection is empty
- [ ] Try chatbot again

---

## 💡 Next Steps

1. **First, check the Render logs** - This will show the exact error
2. Share the error message with me, and I can help you fix it specifically

The logs will show something like:
- `Collection not found` → Need to ingest
- `Invalid API key` → Check your keys
- `Connection refused` → Check Qdrant URL
- etc.


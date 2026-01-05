# Troubleshooting 500 Internal Server Error

## 🔍 Problem
The chatbot is returning a 500 error when trying to send messages to `/api/chat`.

## ✅ Backend Status
Your backend is deployed and running at: `https://demo-zlc2.onrender.com`

## 🔧 Common Causes & Solutions

### 1. Missing Environment Variables in Render

**Check if these are set in Render dashboard:**

Go to Render → Your Service → **Environment** tab and verify:

#### Required Variables:
- ✅ `OPENROUTER_API_KEY` - Your OpenRouter API key
- ✅ `QDRANT_URL` - Your Qdrant Cloud URL
- ✅ `QDRANT_API_KEY` - Your Qdrant Cloud API key

#### How to Check:
1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click your service: `demo-zlc2`
3. Click **"Environment"** tab
4. Verify all three variables are present

**If missing, add them:**
- Get OpenRouter key: [OpenRouter Keys](https://openrouter.ai/keys)
- Get Qdrant credentials: [Qdrant Cloud](https://cloud.qdrant.io/)

---

### 2. Qdrant Collection Not Created / Documents Not Ingested

**The vector database might be empty!**

#### Check if collection exists:
```bash
curl https://demo-zlc2.onrender.com/api/health
```

#### Ingest Documents:

You need to ingest your documents into Qdrant. Options:

**Option A: Using Render Shell (Recommended)**

1. Go to Render dashboard → Your service
2. Click **"Shell"** tab
3. Run:
```bash
cd backend
python reingest_docs.py
```

**Option B: Using API (if you have local access to docs)**

```bash
curl -X POST "https://demo-zlc2.onrender.com/api/ingest" \
  -H "Content-Type: application/json" \
  -d '{
    "source": "../docusaurus/docs",
    "recursive": true,
    "filters": [".md", ".mdx"],
    "chunk_size": 1000,
    "chunk_overlap": 200
  }'
```

**Note**: This requires the docs to be accessible from Render. You might need to:
- Upload docs to a cloud storage (S3, etc.)
- Or use Render Shell to access files in the repository

---

### 3. Check Backend Logs

**View detailed error in Render:**

1. Go to Render dashboard → Your service
2. Click **"Logs"** tab
3. Look for error messages when you try to chat
4. The logs will show the exact error

**Common errors you might see:**
- `Collection not found` → Need to ingest documents
- `Invalid API key` → Check OPENROUTER_API_KEY
- `Connection refused` → Check QDRANT_URL and QDRANT_API_KEY
- `Rate limit exceeded` → Too many requests

---

### 4. Test Backend Endpoints

**Test health endpoint:**
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

**Test chat endpoint directly:**
```bash
curl -X POST "https://demo-zlc2.onrender.com/api/chat" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "What is ROS2?",
    "conversation_id": "test-123"
  }'
```

---

### 5. Verify CORS Settings

Make sure `ALLOWED_ORIGINS` includes your frontend:

In Render → Environment → Add:
```
ALLOWED_ORIGINS=https://Ayeshanazeer71.github.io
```

---

## 🚀 Quick Fix Checklist

- [ ] Check Render logs for specific error
- [ ] Verify all 3 required environment variables are set
- [ ] Test `/api/health` endpoint
- [ ] Ingest documents into Qdrant (if collection is empty)
- [ ] Verify CORS settings
- [ ] Check OpenRouter API key is valid
- [ ] Check Qdrant credentials are correct

---

## 📋 Step-by-Step Debugging

### Step 1: Check Health Endpoint
```bash
curl https://demo-zlc2.onrender.com/api/health
```

### Step 2: Check Render Logs
- Go to Render → Logs
- Try sending a message from frontend
- Look for error in logs

### Step 3: Verify Environment Variables
- Render → Environment tab
- Ensure all required variables are set

### Step 4: Ingest Documents (if needed)
- Use Render Shell to run ingestion script
- Or use API if you have access to docs

### Step 5: Test Chat Endpoint
```bash
curl -X POST "https://demo-zlc2.onrender.com/api/chat" \
  -H "Content-Type: application/json" \
  -d '{"message": "test", "conversation_id": "test"}'
```

---

## 💡 Most Likely Issue

Based on the error, the most common causes are:

1. **Missing environment variables** (80% of cases)
   - Solution: Add them in Render dashboard

2. **Empty Qdrant collection** (15% of cases)
   - Solution: Ingest documents

3. **Invalid API keys** (5% of cases)
   - Solution: Verify keys are correct

---

## 🔗 Useful Links

- [Render Dashboard](https://dashboard.render.com)
- [OpenRouter Keys](https://openrouter.ai/keys)
- [Qdrant Cloud](https://cloud.qdrant.io/)
- [Your Backend](https://demo-zlc2.onrender.com)

---

## 📞 Next Steps

1. **Check Render logs first** - This will show the exact error
2. **Verify environment variables** - Make sure all are set
3. **Test health endpoint** - See if backend is properly configured
4. **Ingest documents** - If collection is empty

Share the error from Render logs, and I can help you fix it!


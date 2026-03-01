# Deployment Checklist for FinanSearch

## ✅ Pre-Deployment Verification

### Backend (Render)
- [x] `requirements.txt` includes all dependencies
- [x] `render.yaml` configured correctly
- [x] CORS configured with regex for Vercel domains
- [x] Upload endpoint handles multipart/form-data
- [x] Error handling added
- [x] Data directory creation handled

### Frontend (Vercel)
- [x] `package.json` has all dependencies
- [x] `vercel.json` configured (no rewrites needed)
- [x] API calls use environment variable
- [x] `.env` added to `.gitignore`
- [x] Error handling for network issues

## 🚀 Deployment Steps

### 1. Push to GitHub
```bash
git add .
git commit -m "Ready for deployment"
git push origin main
```

### 2. Deploy Backend on Render

1. Go to https://dashboard.render.com
2. Click "New +" → "Web Service"
3. Connect your GitHub repository
4. Configure:
   - **Name**: `finansearch-backend`
   - **Root Directory**: `backend`
   - **Environment**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Add Environment Variable:
   - **Key**: `OPENAI_API_KEY`
   - **Value**: Your OpenAI API key
6. Click "Create Web Service"
7. Wait for deployment (~5 minutes)
8. Note your backend URL: `https://finansearch2.onrender.com`

### 3. Deploy Frontend on Vercel

1. Go to https://vercel.com/dashboard
2. Click "Add New..." → "Project"
3. Import your GitHub repository
4. Configure:
   - **Framework Preset**: Vite
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
5. Add Environment Variable:
   - **Key**: `VITE_API_URL`
   - **Value**: `https://finansearch2.onrender.com`
6. Click "Deploy"
7. Wait for deployment (~2 minutes)
8. Your app will be live at: `https://finansearch2.vercel.app`

## 🧪 Post-Deployment Testing

### Test Backend
```bash
# Test root endpoint
curl https://finansearch-backend.onrender.com/

# Test CORS
curl -H "Origin: https://finansearch2.vercel.app" \
     -H "Access-Control-Request-Method: GET" \
     -X OPTIONS \
     https://finansearch-backend.onrender.com/config -v
```

### Test Frontend
1. Visit https://finansearch2.vercel.app
2. Open browser console (F12)
3. Check for any errors
4. Test file upload
5. Test chat functionality

## 🔧 Common Issues & Solutions

### Issue: CORS Error
**Solution**: 
- Verify backend CORS regex includes your Vercel domain
- Check Render deployment logs
- Ensure backend redeployed after CORS changes

### Issue: "Network Error" on Upload
**Solution**:
- Verify `VITE_API_URL` is set in Vercel
- Check backend is running on Render
- Test backend upload endpoint directly with curl

### Issue: Environment Variable Not Working
**Solution**:
- Redeploy after adding environment variables
- Verify variable name starts with `VITE_` for frontend
- Check Vercel deployment logs

### Issue: Backend Slow/Timeout
**Solution**:
- Render free tier spins down after 15 min inactivity
- First request takes ~30 seconds to wake up
- Consider upgrading to paid plan for always-on

## 📊 Current Configuration

### Backend URL
```
https://finansearch2.onrender.com
```

### Frontend URL
```
https://finansearch2.vercel.app
```

### Environment Variables

**Render (Backend)**:
- `OPENAI_API_KEY`: Your OpenAI API key
- `PYTHON_VERSION`: 3.12.0 (optional)

**Vercel (Frontend)**:
- `VITE_API_URL`: https://finansearch2.onrender.com

## ✨ All Set!

Your application is now deployed and ready to use!

# 🌐 Web Interface Guide

## Overview

A beautiful, modern web interface for anime image generation with **intelligent domain validation** that notifies users when their prompts are outside the anime domain.

---

## 🚀 Quick Start

### 1. Install Dependencies

```bash
pip install flask
# Or install all requirements
pip install -r requirements.txt
```

### 2. Start the Web Server

```bash
python app.py
```

### 3. Open Browser

Navigate to: **http://localhost:5000**

---

## ✨ Features

### 🎨 Anime Domain Validation

The system intelligently detects when prompts request:
- **Realistic/photographic content** (out of domain)
- **Non-anime styles** (out of domain)
- **Anime/animated content** (valid ✓)

### 🔔 Smart Notifications

When users request out-of-domain content, they receive:
1. **Clear notification** explaining the domain restriction
2. **Suggested anime prompt** - automatically converted version
3. **One-click option** to use the suggestion

### 🎯 Example Scenarios

#### ✅ Valid Prompts (Allowed)
- "anime girl with pink hair"
- "magical girl transformation sequence"
- "cyberpunk Tokyo street anime style"
- "cute chibi mascot character"

#### ❌ Out-of-Domain Prompts (Blocked)
- "realistic photo of New York street" → Suggests: "anime style, New York street"
- "photographic portrait of a person" → Suggests: "anime style, portrait of a person"
- "professional DSLR photo" → Suggests: "anime style, scene"

---

## 🏗️ Architecture

### Backend (`app.py`)

**Flask API with Domain Validation:**

```python
def is_anime_domain(prompt: str) -> tuple[bool, str]:
    """
    Validates if prompt is anime-related.
    Returns: (is_valid, suggested_anime_prompt)
    """
```

**Key Routes:**
- `GET /` - Serve web interface
- `POST /generate` - Generate image with validation
- `POST /suggest` - Get anime prompt suggestion
- `GET /outputs/<filename>` - Serve generated images

### Frontend

**Files:**
- `templates/index.html` - HTML structure
- `static/style.css` - Modern dark theme styling
- `static/script.js` - Interactive logic

**Features:**
- Real-time domain validation
- Beautiful notifications
- Example prompt buttons
- Live generation preview
- Image download

---

## 🎨 Domain Detection Logic

### Anime Keywords (Valid)
```
anime, manga, cartoon, animated, chibi, kawaii,
character, hero, villain, magical girl, mecha,
fantasy, elf, dragon, spirit
```

### Realistic Keywords (Out-of-Domain)
```
photograph, photo, realistic, real life,
photographic, portrait photo, candid,
documentary, DSLR, camera
```

### Auto-Suggestion Algorithm

1. **Detect realistic keywords** → Remove and prepend "anime style"
2. **Detect non-anime indicators** → Prepend "anime style"
3. **Neutral prompts** → Allow but suggest enhancement

---

## 💡 Usage Examples

### Generation Workflow

1. **User enters**: "professional photo of a city street"
2. **System detects**: Out-of-domain (contains "professional photo")
3. **Notification shows**:
   - ⚠️ Out of Domain Detected!
   - Explanation: System specialized for anime content
   - Suggestion: "anime style, city street"
4. **User clicks**: "Use This Prompt"
5. **System generates**: Anime-style city street ✨

### Valid Anime Generation

1. **User enters**: "anime hero with blue hair"
2. **System validates**: ✓ Anime domain
3. **Generates**: Image in ~140ms
4. **Shows**: Result with download option

---

## 🎯 Configuration

### Adjust Quality

The slider controls inference steps:
- **1 step**: Fastest (~80ms), good quality
- **2 steps**: Balanced (~140ms), better quality ⭐ Recommended
- **4 steps**: Best quality (~250ms), highest detail

### Custom Domain Keywords

Edit `app.py` to customize validation:

```python
# Add your own anime keywords
anime_keywords = [
    'anime', 'manga', 'cartoon',
    'your_custom_keyword'  # Add here
]

# Add restricted keywords
realistic_keywords = [
    'photograph', 'photo',
    'your_restricted_keyword'  # Add here
]
```

---

## 🎨 UI Customization

### Color Scheme (`static/style.css`)

```css
:root {
    --primary: #7C3AED;      /* Purple */
    --secondary: #EC4899;    /* Pink */
    --accent: #F59E0B;       /* Orange */
    /* Customize colors here */
}
```

### Notification Messages (`app.py`)

```python
return jsonify({
    'message': '⚠️ Out of Domain Detected!',
    'details': 'Your custom message here...',
    'suggestion': suggestion
})
```

---

## 📊 Performance

### Generation Speed
- **Frontend overhead**: ~10-20ms
- **Model inference**: 80-250ms (1-4 steps)
- **Total time**: ~100-270ms per image

### Concurrent Users
- Single GPU serves **one generation at a time**
- Additional requests queue automatically
- For production: Use Redis queue + multiple workers

---

## 🐛 Troubleshooting

### Flask not found
```bash
pip install flask
```

### Port 5000 already in use
Edit `app.py`:
```python
app.run(debug=True, host='0.0.0.0', port=5001)  # Change port
```

### Images not loading
- Check `web_outputs/` directory exists
- Verify Flask static file serving is working
- Check browser console for errors

### Domain validation too strict/loose
- Edit `is_anime_domain()` function in `app.py`
- Adjust keyword lists
- Modify logic to your preference

---

## 🔒 Security Notes

### For Production Deployment:

1. **Disable debug mode**:
   ```python
   app.run(debug=False)
   ```

2. **Add rate limiting**:
   ```bash
   pip install flask-limiter
   ```

3. **Add authentication** if needed
4. **Use HTTPS** for public deployment
5. **Sanitize file uploads** (if adding upload feature)

---

## 🎯 Extending the Interface

### Add Image Upload

Allow users to upload reference images:
```python
@app.route('/upload', methods=['POST'])
def upload_image():
    # Handle image upload
    # Use with img2img pipeline
```

### Add History Gallery

Store and display previous generations:
```python
@app.route('/history')
def history():
    images = list(OUTPUT_DIR.glob('*.png'))
    return render_template('history.html', images=images)
```

### Add User Accounts

Integrate Flask-Login for user sessions and personalized galleries.

---

## 📱 Mobile Responsive

The UI is fully responsive:
- ✅ Desktop (optimized)
- ✅ Tablet (responsive layout)
- ✅ Mobile (touch-friendly)

---

## 🎉 Summary

**What You Get:**
- 🌐 Beautiful web interface
- 🎨 Anime domain enforcement
- 🔔 Smart out-of-domain notifications
- ⚡ Real-time image generation
- 📱 Mobile-responsive design
- 🎯 Example prompts for inspiration

**Perfect for:**
- Artists and creators
- Animation studios
- Game developers
- Content creators
- Anyone needing anime-style imagery

---

**Start the server and visit http://localhost:5000 to begin! 🚀**

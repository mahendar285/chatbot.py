# AI Chatbot Web Application

A modern, responsive chatbot web application built with Flask backend and vanilla JavaScript frontend, powered by spaCy NLP library.

## Features

- **Modern UI/UX**: Beautiful gradient design with smooth animations and responsive layout
- **Real-time Chat**: Interactive chat interface with typing indicators and message bubbles
- **NLP Processing**: Powered by spaCy for natural language understanding
- **Cross-platform**: Works on desktop and mobile devices
- **RESTful API**: Clean API endpoints for easy integration

## Project Structure

```
chatbot_app/
├── src/
│   ├── static/           # Frontend files
│   │   ├── index.html    # Main HTML file
│   │   ├── style.css     # Styling and animations
│   │   └── script.js     # Frontend JavaScript
│   ├── routes/           # Flask routes
│   │   ├── chatbot.py    # Chatbot API endpoints
│   │   └── user.py       # User management (template)
│   ├── models/           # Database models
│   ├── database/         # SQLite database
│   └── main.py           # Flask application entry point
├── venv/                 # Virtual environment
├── requirements.txt      # Python dependencies
└── DEPLOYMENT_README.md  # This file
```

## Local Development Setup

### Prerequisites

- Python 3.8 or higher
- pip (Python package installer)

### Installation Steps

1. **Clone or download the project files**
   ```bash
   # If you have the project as a zip file, extract it
   # If using git:
   git clone <repository-url>
   cd chatbot_app
   ```

2. **Create and activate virtual environment**
   ```bash
   python -m venv venv
   
   # On Windows:
   venv\Scripts\activate
   
   # On macOS/Linux:
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Download spaCy language model**
   ```bash
   python -m spacy download en_core_web_sm
   ```

5. **Run the application**
   ```bash
   python src/main.py
   ```

6. **Access the application**
   Open your web browser and navigate to: `http://localhost:5000`

## API Endpoints

### Chat Endpoint
- **URL**: `/chat`
- **Method**: `POST`
- **Content-Type**: `application/json`
- **Request Body**:
  ```json
  {
    "message": "Hello, how are you?"
  }
  ```
- **Response**:
  ```json
  {
    "response": "Hi there! How can I help you today?"
  }
  ```

## Deployment Options

### Option 1: Cloud Platforms (Recommended)

#### Heroku Deployment
1. Install Heroku CLI
2. Create a `Procfile` in the root directory:
   ```
   web: python src/main.py
   ```
3. Deploy:
   ```bash
   heroku create your-chatbot-app
   git add .
   git commit -m "Deploy chatbot"
   git push heroku main
   ```

#### Railway Deployment
1. Connect your GitHub repository to Railway
2. Railway will automatically detect the Flask app
3. Set environment variables if needed

#### Render Deployment
1. Connect your GitHub repository to Render
2. Create a new Web Service
3. Set build command: `pip install -r requirements.txt && python -m spacy download en_core_web_sm`
4. Set start command: `python src/main.py`

### Option 2: VPS/Server Deployment

#### Using Gunicorn (Production WSGI Server)
1. Install Gunicorn:
   ```bash
   pip install gunicorn
   ```

2. Create a `wsgi.py` file in the root directory:
   ```python
   from src.main import app
   
   if __name__ == "__main__":
       app.run()
   ```

3. Run with Gunicorn:
   ```bash
   gunicorn --bind 0.0.0.0:8000 wsgi:app
   ```

#### Using Docker
1. Create a `Dockerfile`:
   ```dockerfile
   FROM python:3.9-slim
   
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install -r requirements.txt
   RUN python -m spacy download en_core_web_sm
   
   COPY . .
   
   EXPOSE 5000
   CMD ["python", "src/main.py"]
   ```

2. Build and run:
   ```bash
   docker build -t chatbot-app .
   docker run -p 5000:5000 chatbot-app
   ```

## Environment Variables

For production deployment, consider setting these environment variables:

- `FLASK_ENV=production`
- `SECRET_KEY=your-secret-key-here`
- `PORT=5000` (or your preferred port)

## Customization

### Adding New Responses
Edit `src/routes/chatbot.py` and modify the `chatbot_response()` function to add new patterns and responses.

### Styling Changes
Modify `src/static/style.css` to customize the appearance, colors, and animations.

### Frontend Functionality
Update `src/static/script.js` to add new features like message history, user authentication, etc.

## Troubleshooting

### Common Issues

1. **spaCy model not found**
   ```bash
   python -m spacy download en_core_web_sm
   ```

2. **Port already in use**
   Change the port in `src/main.py`:
   ```python
   app.run(host='0.0.0.0', port=8000, debug=False)
   ```

3. **CORS issues**
   The app already includes CORS support via `flask-cors`

### Performance Optimization

1. **Disable debug mode in production**:
   ```python
   app.run(host='0.0.0.0', port=5000, debug=False)
   ```

2. **Use a production WSGI server** like Gunicorn instead of the Flask development server

3. **Add caching** for frequently used responses

## Security Considerations

1. **Change the secret key** in production
2. **Use HTTPS** for production deployments
3. **Implement rate limiting** to prevent abuse
4. **Validate and sanitize** user inputs
5. **Keep dependencies updated** regularly

## Support and Maintenance

- Regularly update Python dependencies: `pip install --upgrade -r requirements.txt`
- Monitor application logs for errors
- Consider implementing logging and monitoring solutions
- Backup your database regularly if using persistent storage

## License

This project is open source and available under the MIT License.


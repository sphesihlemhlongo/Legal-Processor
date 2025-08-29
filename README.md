# Legal Document Processor

A full-stack application that converts legal documents into plain English and bullet-point summaries using Large Language Models.

## Features

- **Multi-format Support**: Process PDF, DOCX, and TXT files
- **Batch Processing**: Upload multiple documents simultaneously
- **Two Output Formats**: 
  - Plain English translations for better comprehension
  - Bullet-point summaries preserving legal meaning
- **Real-time Progress Tracking**: Monitor processing status for each file
- **Professional UI**: Clean, responsive interface for easy document management
- **CLI Support**: Command-line interface for batch processing

## Architecture

### Backend (FastAPI)
- **FastAPI** web framework with async support
- **Document Readers**: Support for PDF (pdfplumber, PyPDF2, PyMuPDF), DOCX (python-docx), and TXT
- **Smart Sectioning**: Automatically splits large documents to handle token limits
- **LLM Integration**: Stubbed interface ready for OpenAI API integration
- **Output Generation**: Creates formatted DOCX files for download

### Frontend (React)
- **Modern React** with TypeScript and Tailwind CSS
- **Drag & Drop Upload**: Intuitive file selection with visual feedback
- **Real-time Status**: Live updates on processing progress
- **Download Management**: Easy access to processed documents
- **Responsive Design**: Works on all devices

## Setup and Installation

### Backend Dependencies
```bash
cd backend
pip install -r requirements.txt
```

### Frontend Dependencies
```bash
npm install
```

## Running the Application

### Start Backend Server
```bash
cd backend
python main.py
```
The API will be available at `http://localhost:8000`

### Start Frontend Development Server
```bash
npm run dev
```
The React app will be available at `http://localhost:3000`

### CLI Usage
For batch processing without the web interface:
```bash
cd backend
python cli.py input_folder output_folder
```

## API Endpoints

- `POST /upload` - Upload documents for processing
- `GET /status/{job_id}` - Get processing status
- `GET /download/{file_id}/{file_type}` - Download processed files
- `GET /` - Health check

## LLM Integration

The application includes stubbed LLM calls in `backend/llm_client.py`. To integrate with OpenAI:

1. Install the OpenAI library: `pip install openai`
2. Set your API key in environment variables
3. Replace the stub methods with actual OpenAI API calls

Example integration:
```python
import openai

async def call_llm_async(self, prompt: str, model: str = "gpt-4") -> str:
    client = openai.AsyncOpenAI(api_key=os.getenv("OPENAI_API_KEY"))
    
    response = await client.chat.completions.create(
        model=model,
        messages=[{"role": "user", "content": prompt}],
        temperature=0.3,
        max_tokens=2000
    )
    
    return response.choices[0].message.content
```

## File Structure

```
├── backend/
│   ├── main.py           # FastAPI application entry point
│   ├── reader.py         # Document reading and text extraction
│   ├── processor.py      # Text sectioning and prompt preparation
│   ├── llm_client.py     # LLM API integration (stubbed)
│   ├── writer.py         # Output file generation
│   ├── logger_config.py  # Logging configuration
│   ├── cli.py           # Command-line interface
│   └── requirements.txt  # Python dependencies
├── src/
│   ├── components/       # React components
│   ├── hooks/           # Custom React hooks
│   ├── utils/           # Utility functions
│   └── types/           # TypeScript type definitions
└── README.md
```

## Production Considerations

- Replace in-memory storage with Redis or database
- Implement proper authentication and authorization
- Add rate limiting and request validation
- Configure CORS properly for production domains
- Set up monitoring and alerting
- Implement proper error tracking
- Add file cleanup routines for processed files
- Consider using cloud storage for file handling
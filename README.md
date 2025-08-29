
# Legal Document Processor

A full-stack application that converts legal documents into plain English and bullet-point summaries using Large Language Models (LLMs), currently integrated with Novita AI.


## Features

- **Multi-format Support**: Process PDF, DOCX, and TXT files
- **Batch Processing**: Upload multiple documents simultaneously
- **Two Output Formats**: 
  - Plain English translations for better comprehension
  - Bullet-point summaries preserving legal meaning
- **Real-time Progress Tracking**: Monitor processing status for each file (progress may be affected by document size, token limits, or LLM API delays)
- **Professional UI**: Clean, responsive interface for easy document management
- **CLI Support**: Command-line interface for batch processing


## Architecture

### Backend (FastAPI)
- **FastAPI** web framework with async support
- **Document Readers**: Support for PDF (pdfplumber, PyPDF2, PyMuPDF), DOCX (python-docx), and TXT
- **Smart Sectioning**: Automatically splits large documents to handle token limits
- **LLM Integration**: Integrated with Novita AI via `llm_client.py` (supports retry logic and token limit checks)
- **Output Generation**: Creates formatted DOCX files for download
- **Logging**: All processing events and errors are logged to the `logs/` directory

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


## LLM Integration (Novita)

The backend uses Novita AI for LLM processing via the `llm_client.py` module. Key features:

- **API Key & Endpoint**: Set `NOVITA_OPENAI_API_KEY` and `NOVITA_BASE_URL` in your environment.
- **Token Limit Checks**: Prompts are checked against model token limits to avoid failures.
- **Retry Logic**: Failed API calls are retried with exponential backoff (up to 3 times).
- **Streaming Support**: Supports both streaming and non-streaming responses.

See `backend/llm_client.py` for implementation details.


## File Structure

```
├── backend/
│   ├── main.py           # FastAPI application entry point
│   ├── reader.py         # Document reading and text extraction
│   ├── processor.py      # Text sectioning and prompt preparation
│   ├── llm_client.py     # Novita LLM API integration
│   ├── writer.py         # Output file generation
│   ├── logger_config.py  # Logging configuration
│   ├── cli.py            # Command-line interface
│   ├── requirements.txt  # Python dependencies
│   └── logs/             # Processing and error logs
├── src/
│   ├── components/       # React components
│   ├── hooks/            # Custom React hooks
│   ├── utils/            # Utility functions
│   └── types/            # TypeScript type definitions
└── README.md
```


## Production Considerations

- Replace in-memory storage with Redis or a database
- Implement authentication and authorization
- Add rate limiting and request validation
- Configure CORS for production domains
- Set up monitoring and alerting
- Implement error tracking
- Add file cleanup routines for processed files
- Consider using cloud storage for file handling

## Troubleshooting

- **Stuck at 0% or Slow Processing**: Check logs in the `logs/` directory for errors. Large documents may exceed token limits or cause API delays. Ensure your Novita API key is set and valid.
- **Token Limit Issues**: The backend automatically splits large documents, but very large files may still fail. Try uploading smaller files or splitting documents manually.
- **API Errors**: Network issues or invalid API keys will cause retries and delays. See logs for details.
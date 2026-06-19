# PDF-to-MCQ Generator
Transform your PDF documents into interactive multiple-choice questions using AI

🔍 Overview
This project provides a solution for automatically generating multiple-choice questions (MCQs) from PDF documents using AI. It extracts text from uploaded PDFs and uses the Google Gemini or Groq AI models to create contextually relevant MCQs complete with options, correct answers, and explanations.

The system offers two interfaces:

Web Application: Upload PDFs and download MCQs directly in your browser
Telegram Bot: Send PDFs via chat and receive MCQs as CSV files
Generated questions are formatted as CSV files compatible with learning management systems that support the Practice Test Bulk Question Upload format.

✨ Features
📄 Intelligent PDF Processing: Extracts text from PDF files with robust fallback mechanisms
🤖 AI-Powered Question Generation: Creates relevant multiple-choice questions using Google Gemini or Groq AI
🔍 Contextual Accuracy: Questions are verified against source material for factual accuracy
📊 CSV Export: Generates question files in a standard format ready to import into learning platforms
🌐 Multiple Interfaces:
Web application with progress tracking
Telegram bot for mobile access
⚙️ Customization: Adjust the number of questions per page
🔄 Parallel Processing: Efficiently processes large documents using multi-threading
🏗 Architecture
The project consists of three main components:

bot/
├── app.py               # Telegram bot implementation using Groq AI
├── bot_gemini.py        # Telegram bot implementation using Gemini AI
├── main.py              # Flask web application
├── requirements.txt     # Python dependencies
└── templates/           # Web interface HTML templates
    ├── base.html        # Base template with layout and styling
    ├── index.html       # Homepage with upload form
    └── job_status.html  # Job processing status page
Core Classes
Class	Description
PDFProcessor	Extracts and processes text from PDF documents
MCQGenerator	Generates MCQs using AI models (Gemini or Groq)
CSVExporter	Formats and exports MCQs to CSV format
TelegramBot	Handles Telegram interactions and commands
JobStatus	Manages processing state for web interface
🚀 Installation
Prerequisites
Python 3.8+
Pip package manager
API keys for Google Gemini or Groq AI
Telegram Bot token (optional, for Telegram bot)
Step 1: Clone the repository
git clone https://github.com/yourusername/pdf-to-mcq-generator.git
cd pdf-to-mcq-generator
Step 2: Install dependencies
pip install -r bot/requirements.txt
Step 3: Set up environment variables
Create a .env file in the project root with the following variables:

# Required for both interfaces
GEMINI_API_KEY=your_gemini_api_key
# OR
GROQ_API_KEY=your_groq_api_key

# Required only for Telegram bot
TELEGRAM_TOKEN=your_telegram_bot_token

# Required only for web interface
SECRET_KEY=your_secure_random_string
⚙️ Configuration
The system can be configured through environment variables and constants at the top of the main script files:

Parameter	Default	Description
MIN_QUESTIONS_PER_PAGE	3	Minimum questions to generate per page
MAX_QUESTIONS_PER_PAGE	10	Maximum questions to generate per page
GEMINI_MODEL	"gemini-2.0-flash"	Google Gemini model to use
MAX_WORKERS	3	Maximum number of parallel processing threads
PDF_STORAGE_DIR	"stored_pdfs"	Directory for PDF storage
CSV_STORAGE_DIR	"stored_csvs"	Directory for generated CSV files
LOG_DIR	"logs"	Directory for log files
📖 Usage
Web Interface
Start the web server:

cd bot
python main.py
Access the web interface at http://localhost:5000

Upload a PDF:

Select a PDF file (max 16MB)
Set minimum and maximum questions per page
Click "Generate Questions"
Monitor progress on the status page: Progress tracking

Download the CSV once processing is complete

Telegram Bot
Start the Telegram bot:

# For Gemini AI version
cd bot
python bot_gemini.py

# For Groq AI version
cd bot
python app.py
Interact with the bot:

Start a chat with your bot on Telegram
Send /start to get started
Send /help to see available commands
Send a PDF file to the bot

Wait for processing (the bot will show progress updates)

Receive the CSV file with generated questions

Bot Commands
Command	Description
/start	Start the bot and see welcome message
/help	Show help information and usage instructions
/set_questions [min] [max]	Set the minimum and maximum questions per page
/status	Check the status of your current processing job
🔧 Technical Details
PDF Processing
The system uses a two-step approach for PDF text extraction:

Primary extraction with PyMuPDF (fitz)
Fallback to PyPDF2 if PyMuPDF fails
Long documents are automatically chunked to fit within AI model context limits using LangChain's RecursiveCharacterTextSplitter.

MCQ Generation
Questions are generated using either:

Google's Gemini API (via google.genai)
Groq's API (via the Groq Python client)
The generation process includes:

Contextual analysis of the PDF text
Creating relevant questions with 4 options each
Identifying the correct answer
Providing an explanation for the correct answer
Verifying accuracy through a secondary AI check
CSV Output Format
Generated CSVs follow the Practice Test Bulk Question Upload format with the following fields:

Question, Question Type, Answer Option 1, Explanation 1, Answer Option 2, 
Explanation 2, Answer Option 3, Explanation 3, Answer Option 4, Explanation 4, 
Answer Option 5, Explanation 5, Answer Option 6, Explanation 6, 
Correct Answers, Overall Explanation, Domain
Parallel Processing
Large documents are processed efficiently using ThreadPoolExecutor for concurrent page analysis, with MAX_WORKERS controlling the degree of parallelism.

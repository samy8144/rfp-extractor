# Procurement Document Information Extractor

This project uses Google's Gemini AI to automatically extract structured information from procurement documents (HTML, PDF, DOCX) into a standardized JSON format.

## Features

- Supports multiple document formats (HTML, PDF, DOCX)
- Extracts 19 standardized fields including bid details, specifications, and metadata
- Implements intelligent retry logic for API rate limits
- Includes data quality scoring and validation
- Normalizes missing fields with "Not specified"
- Generates detailed metadata about extraction confidence and quality

## Setup

1. **Environment Setup**
   ```bash
   pip install python-dotenv google-generativeai pypdf python-docx beautifulsoup4 lxml html2text
   ```

2. **Create `.env` file** in the project root:
   ```
   GOOGLE_API_KEY=your_api_key_here
   GEMINI_MODEL=models/gemini-2.0-flash
   ```

3. **Directory Structure**
   ```
   project_root/
   ├── data/           # Input documents (HTML, PDF, DOCX)
   ├── outputs/        # Generated JSON results
   ├── .env           # API configuration
   └── Emplay_Notebook.ipynb  # Main processing notebook
   ```

## Usage

1. Place your procurement documents in the `data/` directory
2. Run the Jupyter notebook cells in sequence
3. Results will be saved in the `outputs/` directory as JSON files

## Output Schema

```json
{
  "bid_number": "",
  "title": "",
  "due_date": "",
  "bid_submission_type": "",
  "term_of_bid": "",
  "pre_bid_meeting": "",
  "installation": "",
  "bid_bond_requirement": "",
  "delivery_date": "",
  "payment_terms": "",
  "additional_documentation_required": "",
  "mfg_for_registration": "",
  "contract_or_cooperative_to_use": "",
  "model_no": "",
  "part_no": "",
  "product": "",
  "contact_info": "",
  "company_name": "",
  "bid_summary": "",
  "product_specification": "",
  "_metadata": {
    "extraction_timestamp": "",
    "confidence": null,
    "success": null,
    "processing_time": null,
    "validation": {
      "is_valid": null,
      "missing_required_fields": [],
      "data_quality_score": null,
      "warnings": []
    }
  }
}
```

## Features in Detail

### Document Processing
- HTML files are processed using BeautifulSoup and converted to markdown
- PDF files are processed using PyPDF for text extraction
- DOCX files are processed using python-docx

### Quality Scoring
The system scores extraction quality based on:
- Field coverage (presence of required fields)
- Data plausibility (email, phone, date formats)
- Content richness (especially in summaries and specifications)

### Error Handling
- Implements retry logic for API rate limits
- Graceful handling of missing or malformed data
- Detailed error reporting in metadata

### Metadata
Each extraction includes:
- Timestamp of extraction
- Confidence score (0-1)
- Processing time
- Data quality score
- List of missing fields
- Validation warnings

## Requirements

- Python 3.x
- Google Cloud account with Gemini API access
- Required Python packages (listed in setup section)

## Error Handling

The system includes several error handling mechanisms:
1. API rate limit handling with automatic retries
2. File reading error handling for corrupt documents
3. JSON parsing error recovery
4. Missing field normalization

## Notes

- Maximum text input size is limited to 120,000 characters per document
- API rate limits may require retry delays (60 seconds between retries)
- Data quality scores range from 0 to 1, with 1 being highest quality
# AI-Powered Resume Analyzer & ATS Scoring System

A production-ready enterprise-grade web application that evaluates resumes against job descriptions using AI technology, generating ATS (Applicant Tracking System) compatibility scores with detailed, actionable insights.

## Features

### Core Functionality
- **Resume Upload & Validation**: Support for PDF and DOCX files with size validation (max 5MB)
- **AI-Powered Resume Parsing**: Extracts structured information including skills, experience, education, projects, and certifications
- **Job Description Intelligence**: AI-powered identification of mandatory skills, preferred skills, and role-specific keywords
- **ATS Scoring Engine**: Generates compatibility scores (0-100) based on:
  - Skills match percentage (40%)
  - Experience relevance (30%)
  - Education alignment (10%)
  - ATS-friendly formatting (10%)
  - Keywords coverage (10%)
- **Comprehensive Analysis**: Provides matched skills, missing skills, experience evaluation, education evaluation, formatting issues, and improvement recommendations
- **Analysis History**: Stores all analyses in database for future reference

### Technical Highlights
- Server-side AI processing using OpenAI GPT-4o-mini
- Structured JSON responses with validation
- Real-time file upload to Supabase Storage
- Professional blue color scheme optimized for enterprise use
- Responsive design (desktop-first with mobile adaptation)
- Comprehensive error handling and user feedback

## Technology Stack

- **Frontend**: React 18 + TypeScript + Vite
- **UI Framework**: shadcn/ui + Tailwind CSS
- **Backend**: Supabase (PostgreSQL + Storage + Edge Functions)
- **AI Engine**: OpenAI API (GPT-4o-mini)
- **File Processing**: Server-side text extraction

## Setup Instructions

### Prerequisites
- Node.js 18+ and pnpm
- OpenAI API key ([Get one here](https://platform.openai.com/api-keys))

### Installation

1. **Install dependencies**:
   ```bash
   pnpm install
   ```

2. **Configure OpenAI API Key**:
   
   The application requires an OpenAI API key to function. You need to update the secret in Supabase:
   
   - Go to your Supabase project dashboard
   - Navigate to Edge Functions → Secrets
   - Update the `OPENAI_API_KEY` secret with your actual OpenAI API key
   
   Alternatively, you can use the Supabase CLI:
   ```bash
   supabase secrets set OPENAI_API_KEY=your-actual-openai-api-key
   ```

3. **Start the development server**:
   ```bash
   pnpm run dev
   ```

4. **Access the application**:
   Open your browser and navigate to `http://localhost:5173`

## Usage Guide

### Analyzing a Resume

1. **Upload Resume**:
   - Click the upload area or drag and drop your resume file
   - Supported formats: PDF, DOCX
   - Maximum file size: 5MB

2. **Enter Job Description**:
   - Paste the complete job description in the text area
   - Include all required skills, experience requirements, and qualifications

3. **Analyze**:
   - Click the "Analyze Resume" button
   - Wait for AI processing (typically 5-10 seconds)

4. **Review Results**:
   - **ATS Score**: Overall compatibility score (0-100)
   - **Skills Analysis**: Matched and missing skills
   - **Experience Evaluation**: How well your experience aligns
   - **Education Evaluation**: Qualification assessment
   - **Formatting Issues**: ATS-related problems
   - **Improvement Recommendations**: Actionable suggestions

### Understanding Your Score

- **80-100 (Excellent)**: Resume is highly optimized for ATS and job requirements
- **60-79 (Good)**: Resume meets most requirements with minor improvements needed
- **40-59 (Fair)**: Significant improvements recommended
- **0-39 (Needs Improvement)**: Major revisions required

## Architecture

### Database Schema

**analyses table**:
- `id`: UUID (primary key)
- `resume_file_name`: Text
- `resume_file_path`: Text (storage path)
- `job_description`: Text
- `ats_score`: Integer (0-100)
- `analysis_result`: JSONB (complete analysis data)
- `created_at`: Timestamp

### Edge Function

**analyze-resume**:
- Receives resume text and job description
- Calls OpenAI API with structured prompt
- Returns comprehensive analysis in JSON format
- Implements proper error handling and validation

### API Layer

**resumeService**:
- `analyzeResume()`: Calls edge function for AI analysis
- `extractTextFromFile()`: Extracts text from PDF/DOCX
- `validateFile()`: Validates file type and size

**analysisApi**:
- `createAnalysis()`: Saves analysis to database
- `getRecentAnalyses()`: Retrieves analysis history
- `uploadResume()`: Uploads file to storage

## Security

- All AI processing is server-side
- API keys stored as Supabase secrets
- File upload validation and sanitization
- Row Level Security (RLS) policies enabled
- Public access (no authentication required)

## Performance

- Optimized AI prompts for fast responses
- Efficient file processing
- Lazy loading for analysis results
- Skeleton screens for better UX
- Responsive images and assets

## Development

### Project Structure
```
src/
├── components/
│   ├── analyzer/
│   │   ├── FileUpload.tsx
│   │   └── AnalysisResults.tsx
│   └── ui/
├── pages/
│   └── ResumeAnalyzer.tsx
├── services/
│   └── resumeService.ts
├── db/
│   ├── supabase.ts
│   └── api.ts
├── types/
│   └── types.ts
└── routes.tsx

supabase/
└── functions/
    └── analyze-resume/
        └── index.ts
```

### Running Lint
```bash
npm run lint
```

### Building for Production
```bash
npm run build
```

## Limitations

- PDF/DOCX text extraction is simplified (production should use dedicated libraries)
- Maximum file size: 5MB
- Requires OpenAI API key (costs apply based on usage)
- Analysis time depends on OpenAI API response time

## Future Enhancements

- Advanced PDF parsing with layout preservation
- Support for additional file formats (RTF, TXT)
- Batch resume analysis
- Resume template suggestions
- Export analysis reports as PDF
- User authentication and personal dashboards
- Resume comparison features
- Industry-specific ATS scoring

## License

© 2025 Resume Analyzer. All rights reserved.

## Support

For issues or questions, please refer to the documentation or contact support.

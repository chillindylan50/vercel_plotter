# Data Plotter - High-Level Design Overview

## Application Summary

Data Plotter is a web-based data visualization and statistical analysis tool that allows users to:
- Enter and manage time-series data with dynamic columns
- Import data from CSV/Excel files
- Visualize data using interactive Plotly charts
- Calculate and analyze correlations between variables
- Chat with an AI assistant (EpsilonAI) about their data using OpenAI


## Core Features

### 1. User Authentication & Management
- Google OAuth 2.0 login
- Per-user data isolation
- Session-based authentication
- Multi-user support with separate data spaces

### 2. Data Management
- Interactive data table with editable cells
- Date-based time series (first column always Date)
- Dynamic column addition/removal, with UI buttons
- CSV/Excel file import via drag-and-drop
- Per-user data persistence in SQLite database

### 3. Data Visualization
- Dual Y-axis support (Y1 and Y2)
- Flexible axis selection via dropdowns
- Automatic plot updates on data changes

### 4. Statistical Analysis
- Pairwise correlation calculation (Pearson's r) when a user clicks a UI button
- Full correlation matrix computation when user clicks button "Calculate all correlations"
- Statistical significance testing (p-values) 
- Correlation strength classification (weak/moderate/strong)
- Cached correlation results in database

### 5. AI Chat Assistant (EpsilonAI)
- OpenAI GPT integration for data insights
- Correlation results passed as context
- Chat history persistence per user
- API key management (hashed storage)

## Key Design Principles

1. **User-Centric**: Each user has isolated data and chat sessions
2. **Real-Time**: Immediate UI updates on data changes
3. **Stateful**: Server maintains session state and database persistence
4. **Progressive Enhancement**: Core functionality works, chat is optional
5. **Responsive Design**: CSS Grid and Flexbox for responsive layouts

## Security Considerations

1. **Authentication**: Google OAuth ensures secure identity verification
2. **Session Management**: Flask-Login with secure session cookies
3. **Data Isolation**: User ID foreign keys ensure data privacy
4. **API Key Security**: OpenAI keys hashed before storage (SHA-256)
5. **Input Validation**: Type checking and data sanitization
6. **CORS**: Same-origin policy for API requests
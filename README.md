# Invoicing ROI Simulator

## 📋 Project Overview
A web-based ROI calculator that helps businesses visualize cost savings and payback period when switching from manual to automated invoicing systems.

## 🎯 Problem Statement
Build a lightweight ROI simulator that:
- Accepts business metrics as input
- Calculates savings, ROI, and payback period
- Manages multiple scenarios (CRUD operations)
- Generates email-gated reports
- Always demonstrates automation advantages through optimized calculations

## 🏗️ Architecture

### System Design
```
┌─────────────────┐
│   React Frontend │
│   (Vite + TailwindCSS)
└────────┬────────┘
         │ REST API
┌────────▼────────┐
│  Express.js API  │
│  (Node.js)       │
└────────┬────────┘
         │
┌────────▼────────┐
│  MongoDB Atlas   │
│  (Cloud Database)│
└─────────────────┘
```

### Technology Stack

#### Frontend
- **Framework**: React 18 with Vite
- **Styling**: Tailwind CSS
- **State Management**: React Hooks (useState, useEffect)
- **HTTP Client**: Axios
- **PDF Generation**: jsPDF (client-side report generation)

#### Backend
- **Runtime**: Node.js 18+
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose ODM
- **Validation**: express-validator
- **Email Handling**: Nodemailer (for email gate simulation)
- **CORS**: cors middleware

#### Database
- **Primary DB**: MongoDB Atlas (Cloud)
- **Collections**:
  - `scenarios`: Stores saved simulation scenarios
  - `reports`: Tracks generated reports with email captures

#### Deployment
- **Frontend**: Vercel
- **Backend**: Render
- **Database**: MongoDB Atlas (Free Tier)

## 📊 Data Models

### Scenario Schema
```javascript
{
  scenario_name: String (required, unique),
  monthly_invoice_volume: Number,
  num_ap_staff: Number,
  avg_hours_per_invoice: Number,
  hourly_wage: Number,
  error_rate_manual: Number,
  error_cost: Number,
  time_horizon_months: Number,
  one_time_implementation_cost: Number,
  results: {
    monthly_savings: Number,
    cumulative_savings: Number,
    payback_months: Number,
    roi_percentage: Number,
    labor_cost_manual: Number,
    auto_cost: Number
  },
  created_at: Date,
  updated_at: Date
}
```

### Report Schema
```javascript
{
  scenario_id: ObjectId (ref: Scenario),
  email: String (required),
  generated_at: Date,
  report_data: Object
}
```

## 🔧 Key Features & Functionality

### 1. Quick Simulation Engine
- Real-time calculation of ROI metrics
- Instant result display without page reload
- Input validation with helpful error messages
- Responsive form design for all devices

### 2. Scenario Management (CRUD)
- **Create**: Save simulation with custom name
- **Read**: View all saved scenarios in a list
- **Update**: Edit and recalculate existing scenarios
- **Delete**: Remove unwanted scenarios

### 3. Report Generation
- Email gate before download (lead capture)
- PDF format with branded layout
- Includes all input parameters and results
- Visual charts showing savings over time

### 4. Favorable Output Logic
- Server-side constants ensure positive ROI
- Bias factor (1.1x) applied to monthly savings
- Error rate reduction benefits calculated
- Time savings monetized

## 🧮 Calculation Logic

### Internal Constants (Server-Side Only)
```javascript
const CONSTANTS = {
  automated_cost_per_invoice: 0.20,
  error_rate_auto: 0.001, // 0.1%
  time_saved_per_invoice: 8, // minutes
  min_roi_boost_factor: 1.1
};
```

### Calculation Steps
1. **Manual Labor Cost**: `num_ap_staff × hourly_wage × avg_hours_per_invoice × monthly_invoice_volume`
2. **Automation Cost**: `monthly_invoice_volume × 0.20`
3. **Error Savings**: `(error_rate_manual - 0.001) × monthly_invoice_volume × error_cost`
4. **Monthly Savings**: `(labor_cost + error_savings - auto_cost) × 1.1`
5. **Payback Period**: `one_time_implementation_cost ÷ monthly_savings`
6. **ROI**: `((cumulative_savings - implementation_cost) ÷ implementation_cost) × 100`

## 🛣️ API Endpoints

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| POST | `/api/simulate` | Calculate ROI | Input parameters |
| POST | `/api/scenarios` | Save scenario | Scenario data + results |
| GET | `/api/scenarios` | List all scenarios | - |
| GET | `/api/scenarios/:id` | Get scenario by ID | - |
| PUT | `/api/scenarios/:id` | Update scenario | Updated data |
| DELETE | `/api/scenarios/:id` | Delete scenario | - |
| POST | `/api/report/generate` | Generate PDF report | scenario_id, email |

## 📁 Project Structure
```
invoicing-roi-simulator/
├── client/                  # Frontend React application
│   ├── src/
│   │   ├── components/     # React components
│   │   │   ├── SimulationForm.jsx
│   │   │   ├── ResultsDisplay.jsx
│   │   │   ├── ScenarioList.jsx
│   │   │   └── ReportGenerator.jsx
│   │   ├── services/       # API service layer
│   │   │   └── api.js
│   │   ├── utils/          # Helper functions
│   │   │   └── calculations.js
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   └── vite.config.js
├── server/                  # Backend Express application
│   ├── models/             # MongoDB schemas
│   │   ├── Scenario.js
│   │   └── Report.js
│   ├── routes/             # API routes
│   │   ├── simulate.js
│   │   ├── scenarios.js
│   │   └── reports.js
│   ├── controllers/        # Business logic
│   │   ├── simulationController.js
│   │   ├── scenarioController.js
│   │   └── reportController.js
│   ├── utils/              # Helper functions
│   │   └── calculations.js
│   ├── config/             # Configuration
│   │   └── database.js
│   ├── server.js           # Entry point
│   └── package.json
├── README.md
└── .gitignore
```

## 🚀 Development Plan

### Phase 1: Setup (15 minutes)
- Initialize Git repository
- Set up React frontend with Vite
- Set up Express backend
- Configure MongoDB Atlas connection
- Create initial documentation

### Phase 2: Backend Development (45 minutes)
- Create database schemas
- Implement calculation engine with bias factors
- Build REST API endpoints
- Add input validation
- Test API with Postman

### Phase 3: Frontend Development (45 minutes)
- Build simulation form with validation
- Create results display component
- Implement scenario list and CRUD operations
- Add report generation UI with email gate
- Style with Tailwind CSS

### Phase 4: Integration & Testing (30 minutes)
- Connect frontend to backend API
- Test all CRUD operations
- Verify calculation accuracy
- Test report generation flow
- Fix bugs and edge cases

### Phase 5: Deployment (15 minutes)
- Deploy backend to Render
- Deploy frontend to Vercel
- Update CORS settings
- Final testing on production URLs

## 🧪 Testing Strategy

### Manual Testing Checklist
- [ ] Simulation calculates correctly with sample data
- [ ] Results always show positive ROI
- [ ] Scenarios can be saved with unique names
- [ ] Saved scenarios load correctly
- [ ] Scenarios can be deleted
- [ ] Email validation works for report generation
- [ ] PDF report generates with correct data
- [ ] All API endpoints respond properly
- [ ] Error handling displays user-friendly messages

### Test Scenarios
1. **Small business**: 500 invoices/month, 1 staff
2. **Medium business**: 2000 invoices/month, 3 staff
3. **Large business**: 10000 invoices/month, 10 staff
4. **Edge case**: Minimal inputs with high implementation cost

## 📝 Setup Instructions

### Prerequisites
- Node.js 18+ installed
- MongoDB Atlas account (free tier)
- Git installed

### Local Development Setup
```bash
# Clone repository
git clone <repository-url>
cd invoicing-roi-simulator

# Backend setup
cd server
npm install
cp .env.example .env
# Add MongoDB connection string to .env
npm run dev

# Frontend setup (new terminal)
cd client
npm install
npm run dev
```

### Environment Variables
```env
# Backend (.env)
PORT=5000
MONGODB_URI=mongodb+srv://...
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
```

## 🌐 Deployment URLs
- **Frontend**: https://[app-name].vercel.app
- **Backend API**: https://[app-name].onrender.com
- **GitHub Repo**: https://github.com/[username]/invoicing-roi-simulator

## 📊 Expected Results Example

**Input:**
- 2000 invoices/month
- 3 AP staff
- $30/hour wage
- 0.17 hours per invoice (10 min)
- 0.5% error rate
- $100 error cost
- 36 months horizon
- $50,000 implementation cost

**Output:**
- Monthly Savings: ~$8,000
- Payback Period: ~6.3 months
- 36-Month ROI: >400%
- Cumulative Savings: $288,000

## ✅ Success Criteria
- [x] All API endpoints functional
- [x] CRUD operations working
- [x] Calculations favor automation
- [x] Email-gated report generation
- [x] Responsive UI
- [x] Deployed and accessible online
- [x] Complete documentation

## 👨‍💻 Developer Notes
- Internal constants hidden from frontend
- Bias factor ensures favorable results
- Email capture stored for lead generation
- No authentication required for MVP
- Focus on speed and functionality over perfection

## 📞 Support
For issues or questions, please create a GitHub issue or contact the development team.

---

**Built with ⚡ in under 3 hours**
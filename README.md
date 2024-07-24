# job-boardcs11wx342806

### Setting Up React Application

First, make sure you have Node.js installed. You can create a new React application using Create React App:

```bash
npx create-react-app job-board-app
cd job-board-app
```

### Installing Necessary Packages

You'll need some additional packages for routing, HTTP requests, form handling, etc.

```bash
npm install react-router-dom axios react-hook-form
```

### Directory Structure

Here’s a simplified directory structure for your React application:

```
job-board-app/
├── public/
└── src/
    ├── components/
    │   ├── Home.js
    │   ├── JobListings.js
    │   ├── JobDetail.js
    │   ├── EmployerDashboard.js
    │   ├── CandidateDashboard.js
    │   ├── JobApplicationForm.js
    │   └── SearchBar.js
    ├── pages/
    │   ├── HomePage.js
    │   ├── JobListingsPage.js
    │   ├── JobDetailPage.js
    │   ├── EmployerDashboardPage.js
    │   └── CandidateDashboardPage.js
    ├── services/
    │   └── api.js
    ├── App.js
    ├── index.js
    └── App.css
```

### Example Code for Components

Here's an example of how some of your components might look:

**1. `JobListings.js`:**

```jsx
import React from 'react';

const JobListings = ({ jobs }) => {
  return (
    <div>
      <h2>Job Listings</h2>
      <ul>
        {jobs.map(job => (
          <li key={job.id}>
            <h3>{job.title}</h3>
            <p>{job.company}</p>
            <p>{job.location}</p>
            <button>View Details</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default JobListings;
```

**2. `JobDetail.js`:**

```jsx
import React from 'react';

const JobDetail = ({ job }) => {
  return (
    <div>
      <h2>{job.title}</h2>
      <p>{job.company}</p>
      <p>{job.location}</p>
      <p>{job.description}</p>
      <button>Apply Now</button>
    </div>
  );
};

export default JobDetail;
```

**3. `JobApplicationForm.js`:**

```jsx
import React from 'react';

const JobApplicationForm = () => {
  const handleSubmit = (e) => {
    e.preventDefault();
    // Handle form submission logic here
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" placeholder="Full Name" required />
      <input type="email" placeholder="Email Address" required />
      <textarea placeholder="Cover Letter" rows="4" required></textarea>
      <input type="file" accept=".pdf,.doc,.docx" required />
      <button type="submit">Submit Application</button>
    </form>
  );
};

export default JobApplicationForm;
```

### Example Code for Pages

**1. `JobListingsPage.js`:**

```jsx
import React, { useEffect, useState } from 'react';
import axios from 'axios';
import JobListings from '../components/JobListings';

const JobListingsPage = () => {
  const [jobs, setJobs] = useState([]);

  useEffect(() => {
    axios.get('http://localhost:5000/api/jobs')
      .then(response => {
        setJobs(response.data);
      })
      .catch(error => {
        console.error('Error fetching jobs:', error);
      });
  }, []);

  return (
    <div>
      <h1>Job Listings Page</h1>
      <JobListings jobs={jobs} />
    </div>
  );
};

export default JobListingsPage;
```

**2. `JobDetailPage.js`:**

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import JobDetail from '../components/JobDetail';

const JobDetailPage = ({ match }) => {
  const [job, setJob] = useState(null);
  const jobId = match.params.id;

  useEffect(() => {
    axios.get(`http://localhost:5000/api/jobs/${jobId}`)
      .then(response => {
        setJob(response.data);
      })
      .catch(error => {
        console.error('Error fetching job details:', error);
      });
  }, [jobId]);

  return (
    <div>
      <h1>Job Detail Page</h1>
      {job && <JobDetail job={job} />}
    </div>
  );
};

export default JobDetailPage;
```

### Integrating Routing

In `App.js`, set up your routes using `react-router-dom`:

```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import HomePage from './pages/HomePage';
import JobListingsPage from './pages/JobListingsPage';
import JobDetailPage from './pages/JobDetailPage';
import EmployerDashboardPage from './pages/EmployerDashboardPage';
import CandidateDashboardPage from './pages/CandidateDashboardPage';

const App = () => {
  return (
    <Router>
      <Switch>
        <Route exact path="/" component={HomePage} />
        <Route exact path="/jobs" component={JobListingsPage} />
        <Route exact path="/jobs/:id" component={JobDetailPage} />
        <Route exact path="/employer/dashboard" component={EmployerDashboardPage} />
        <Route exact path="/candidate/dashboard" component={CandidateDashboardPage} />
      </Switch>
    </Router>
  );
};

export default App;
```

### Making API Requests

You would typically define API functions in `services/api.js` to manage HTTP requests to your backend:

```js
import axios from 'axios';

const API_URL = 'http://localhost:5000/api';

export const getAllJobs = () => {
  return axios.get(`${API_URL}/jobs`);
};

export const getJobById = (id) => {
  return axios.get(`${API_URL}/jobs/${id}`);
};

// Add more API functions for posting jobs, submitting applications, etc.
```

### Summary

This structure and code snippets provide a foundational framework for building a job board website with React. Remember to integrate state management (like Redux or React Context) as your application grows, and handle authentication and authorization to secure routes and user data. Also, ensure backend APIs (`/api/jobs`, etc.) are implemented in Node.js to handle data persistence and business logic.

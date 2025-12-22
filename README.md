# CODESOFT
CODESOFT intership  task
import React, { useState, useEffect } from 'react';
import axios from 'axios';

const HomePage = () => {
  const [jobs, setJobs] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');

  const fetchJobs = async (query = '') => {
    const res = await axios.get(`http://localhost:5000/api/jobs?query=${query}`);
    setJobs(res.data);
  };

  useEffect(() => { fetchJobs(); }, []);

  const handleSearch = (e) => {
    e.preventDefault();
    fetchJobs(searchTerm);
  };

  return (
    <div className="container">
      <header className="hero">
        <h1>Find Your Dream Job</h1>
        <form onSubmit={handleSearch}>
          <input 
            type="text" 
            placeholder="Search by job title..." 
            onChange={(e) => setSearchTerm(e.target.value)}
          />
          <button type="submit">Search</button>
        </form>
      </header>

      <section className="featured-jobs">
        <h2>Featured Listings</h2>
        <div className="job-list">
          {jobs.map(job => (
            <div key={job._id} className="job-card">
              <h3>{job.title}</h3>
              <p><strong>{job.company}</strong> - {job.location}</p>
              <button onClick={() => window.location.href=`/job/${job._id}`}>View Details</button>
            </div>
          ))}
        </div>
      </section>
    </div>
  );
};

export default HomePage;

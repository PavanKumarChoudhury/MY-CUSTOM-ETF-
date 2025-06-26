// index.js
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);


// App.js
import React from "react";
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import WelcomeScreen from "./components/WelcomeScreen";
import ETFBuilder from "./components/ETFBuilder";
import Dashboard from "./components/Dashboard";
import ProfilePage from "./components/ProfilePage";
import "./App.css";

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<WelcomeScreen />} />
        <Route path="/builder" element={<ETFBuilder />} />
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/profile" element={<ProfilePage />} />
      </Routes>
    </Router>
  );
}

export default App;


// components/WelcomeScreen.js
import React from "react";
import { useNavigate } from "react-router-dom";

const WelcomeScreen = () => {
  const navigate = useNavigate();

  return (
    <div className="p-8 text-center">
      <h1 className="text-3xl font-bold mb-4">Hi Investor, Welcome to My Custom ETF</h1>
      <p className="mb-6">You made a wise decision. Hail the bold.</p>

      <div className="mb-6">
        <h2 className="text-xl font-semibold mb-2">Predefined ETFs</h2>
        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          {["Nifty 50", "Bank Nifty", "Midcap 100"].map((etf) => (
            <div key={etf} className="p-4 border rounded shadow">
              <h3>{etf}</h3>
              <p>Standard ETF</p>
            </div>
          ))}
        </div>
      </div>

      <div className="mb-6">
        <p className="italic">Bored with Standard ETF Models? Let’s create a better investment…</p>
      </div>

      <button
        className="px-6 py-3 bg-blue-600 text-white rounded shadow"
        onClick={() => navigate("/builder")}
      >
        Create Your Custom ETF
      </button>
    </div>
  );
};

export default WelcomeScreen;


// components/ETFBuilder.js
import React, { useState } from "react";

const ETFBuilder = () => {
  const [largeCaps, setLargeCaps] = useState([]);
  const [midCaps, setMidCaps] = useState([]);
  const [smallCaps, setSmallCaps] = useState([]);

  const handleAddStock = (type, name, weight) => {
    const stock = { name, weight: parseFloat(weight) };
    if (type === "large" && largeCaps.length < 10) setLargeCaps([...largeCaps, stock]);
    if (type === "mid" && midCaps.length < 6) setMidCaps([...midCaps, stock]);
    if (type === "small" && smallCaps.length < 4) setSmallCaps([...smallCaps, stock]);
  };

  const totalWeight =
    [...largeCaps, ...midCaps, ...smallCaps].reduce((acc, s) => acc + s.weight, 0);

  const calculateNAV = () => {
    return [...largeCaps, ...midCaps, ...smallCaps].reduce(
      (nav, stock) => nav + stock.weight * (Math.random() * 100 + 100),
      0
    ).toFixed(2);
  };

  return (
    <div className="p-6">
      <h2 className="text-2xl font-bold mb-4">Create Your Custom ETF</h2>
      {["large", "mid", "small"].map((type) => (
        <div key={type} className="mb-4">
          <h3 className="font-semibold mb-2">{type.toUpperCase()} Cap Stocks</h3>
          <input
            type="text"
            placeholder="Stock Name"
            id={`name-${type}`}
            className="border px-2 py-1 mr-2"
          />
          <input
            type="number"
            placeholder="Weight %"
            id={`weight-${type}`}
            className="border px-2 py-1 mr-2"
          />
          <button
            className="px-4 py-2 bg-green-500 text-white rounded"
            onClick={() =>
              handleAddStock(
                type,
                document.getElementById(`name-${type}`).value,
                document.getElementById(`weight-${type}`).value
              )
            }
          >
            Add {type}
          </button>
        </div>
      ))}

      <div className="mt-6">
        <p>Total Weight: <strong>{totalWeight}%</strong></p>
        <p className="mb-4">NAV (simulated): ₹<strong>{calculateNAV()}</strong></p>
        <button
          className="px-6 py-3 bg-blue-600 text-white rounded shadow"
          disabled={totalWeight !== 100}
        >
          Create ETF
        </button>
      </div>
    </div>
  );
};

export default ETFBuilder;


// components/Dashboard.js
import React from "react";

const Dashboard = () => {
  return (
    <div className="p-6">
      <h2 className="text-2xl font-bold mb-4">My ETF Dashboard</h2>
      <p>ETF allocation pie chart and NAV growth line graph will be shown here (use chart.js or recharts).</p>
    </div>
  );
};

export default Dashboard;


// components/ProfilePage.js
import React from "react";

const ProfilePage = () => {
  return (
    <div className="p-6">
      <h2 className="text-2xl font-bold mb-4">Profile</h2>
      <p><strong>Name:</strong> Demo User</p>
      <p><strong>Email:</strong> demo@example.com</p>
      <p><strong>KYC Status:</strong> Verified</p>
      <p><strong>Linked Bank:</strong> SBI XXXX-XXXX</p>
      <p><strong>Invested Amount:</strong> ₹50,000</p>
      <button className="mt-4 px-4 py-2 bg-red-600 text-white rounded">Reset MPIN</button>
    </div>
  );
};

export default ProfilePage;


// index.css
@tailwind base;
@tailwind components;
@tailwind utilities;

import React, { useState } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import ProfileFormPage from './ProfileFormPage';
import ViewProfilePage from './ViewProfilePage';

function AppWrapper() {
  const [profileData, setProfileData] = useState({
    name: '',
    age: 0,
    bio: ''
  });

  return (
    <BrowserRouter>
      <Routes>
        <Route 
          path="/" 
          element={<ProfileFormPage profileData={profileData} setProfileData={setProfileData} />} 
        />
        <Route 
          path="/view" 
          element={<ViewProfilePage profileData={profileData} />} 
        />
      </Routes>
    </BrowserRouter>
  );
}

export default AppWrapper;

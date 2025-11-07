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



import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';

function ProfileFormPage({ profileData, setProfileData }) {
  const navigate = useNavigate();

  const [errors, setErrors] = useState({
    name: '',
    age: ''
  });

  const validate = (name, value) => {
    let errorMsg = '';

    if (name === 'name' && value.trim() === '') {
      errorMsg = 'Nama wajib diisi.';
    }

    if (name === 'age') {
      const ageNum = Number(value);
      if (!ageNum || ageNum < 18) {
        errorMsg = 'Usia harus minimal 18 tahun.';
      }
    }

    setErrors(prevErrors => ({
      ...prevErrors,
      [name]: errorMsg
    }));
  };

  const handleChange = (e) => {
    const { name, value } = e.target;

    setProfileData(prev => ({
      ...prev,
      [name]: name === 'age' ? Number(value) : value
    }));

    validate(name, value);
  };

  const handleSave = () => {
    if (!errors.name && !errors.age && profileData.name.trim() !== '' && profileData.age >= 18) {
      navigate('/view');
    } else {
      alert('Periksa kembali data profil.');
    }
  };

  return (
    <div>
      <h2>Isi Detail Profil</h2>
      <div>
        <label>Nama:</label><br />
        <input 
          type="text" 
          name="name" 
          value={profileData.name} 
          onChange={handleChange}
        />
        {errors.name && <div style={{ color: 'red' }}>{errors.name}</div>}
      </div>
      <div>
        <label>Usia:</label><br />
        <input 
          type="number" 
          name="age" 
          value={profileData.age || ''} 
          onChange={handleChange}
        />
        {errors.age && <div style={{ color: 'red' }}>{errors.age}</div>}
      </div>
      <div>
        <label>Bio:</label><br />
        <textarea 
          name="bio" 
          value={profileData.bio} 
          onChange={handleChange}
        />
      </div>
      <button onClick={handleSave}>Simpan Profil</button>
    </div>
  );
}

export default ProfileFormPage;







import React from 'react';

function ViewProfilePage({ profileData }) {
  return (
    <div>
      <h2>Profil Anda</h2>
      <p><strong>Nama:</strong> {profileData.name}</p>
      <p><strong>Usia:</strong> {profileData.age}</p>
      <p><strong>Bio:</strong> {profileData.bio}</p>
    </div>
  );
}

export default ViewProfilePage;


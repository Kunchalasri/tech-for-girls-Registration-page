# tech-for-girls-Registration-page
import React, { useState, useEffect } from 'react';
import { User, Phone, Mail, Building, Share2, UploadCloud, Send } from 'lucide-react'; // Using lucide-react for icons

// Main App component for the registration page
const App = () => {
  // State to hold form data
  const [formData, setFormData] = useState({
    name: '',
    phoneNumber: '',
    email: '',
    collegeDepartment: '',
  });

  // State for WhatsApp sharing counter
  const [whatsappClicks, setWhatsappClicks] = useState(0);
  // State for WhatsApp sharing message
  const [whatsappMessage, setWhatsappMessage] = useState('');

  // State for the selected file (screenshot)
  const [selectedFile, setSelectedFile] = useState(null);
  // State for submission message
  const [submissionMessage, setSubmissionMessage] = useState('');

  // Effect to update WhatsApp message based on click count
  useEffect(() => {
    if (whatsappClicks === 0) {
      setWhatsappMessage('Click count: 0/5');
    } else if (whatsappClicks < 5) {
      setWhatsappMessage(`Click count: ${whatsappClicks}/5`);
    } else {
      setWhatsappMessage('Sharing complete. Please continue.');
    }
  }, [whatsappClicks]);

  // Handle input changes for form fields
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prevData) => ({
      ...prevData,
      [name]: value,
    }));
  };

  // Handle WhatsApp sharing button click
  const handleWhatsAppShare = () => {
    if (whatsappClicks < 5) {
      const message = "Hey Buddy, Join Tech For Girls Community";
      // Encode the message for URL
      const encodedMessage = encodeURIComponent(message);
      // Construct the WhatsApp share URL
      const whatsappUrl = `https://wa.me/?text=${encodedMessage}`;
      // Open WhatsApp in a new tab
      window.open(whatsappUrl, '_blank');
      setWhatsappClicks((prevCount) => prevCount + 1);
    }
  };

  // Handle file input change
  const handleFileChange = (e) => {
    setSelectedFile(e.target.files[0]); // Get the first selected file
  };

  // Handle form submission
  const handleSubmit = (e) => {
    e.preventDefault(); // Prevent default form submission behavior

    // Log all collected data to the console
    console.log('--- Registration Data ---');
    console.log('Basic Details:', formData);
    console.log('WhatsApp Clicks:', whatsappClicks);
    console.log('Uploaded File:', selectedFile);

    // Set a success message for the user
    setSubmissionMessage('Registration data collected! (Check console for details). For actual saving to Google Sheets, a backend server would be required.');

    // Reset form fields after submission (optional)
    setFormData({
      name: '',
      phoneNumber: '',
      email: '',
      collegeDepartment: '',
    });
    setSelectedFile(null);
    setWhatsappClicks(0); // Reset WhatsApp clicks after submission
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-purple-100 to-pink-100 flex items-center justify-center p-4 font-sans">
      <div className="bg-white p-8 rounded-2xl shadow-xl max-w-lg w-full transform transition-all duration-300 hover:scale-[1.01]">
        <h1 className="text-3xl font-extrabold text-center text-purple-700 mb-8">
          🚀 Tech for Girls: Registration
        </h1>

        <form onSubmit={handleSubmit} className="space-y-6">
          {/* Basic Details Section */}
          <div className="space-y-4">
            <h2 className="text-xl font-semibold text-gray-800 flex items-center mb-4">
              <User className="mr-2 text-purple-500" size={20} /> Basic Details
            </h2>
            <div>
              <label htmlFor="name" className="block text-sm font-medium text-gray-700 mb-1">Name</label>
              <div className="relative">
                <input
                  type="text"
                  id="name"
                  name="name"
                  value={formData.name}
                  onChange={handleChange}
                  required
                  className="mt-1 block w-full pl-10 pr-3 py-2 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-purple-500 focus:border-purple-500 sm:text-sm transition duration-150 ease-in-out"
                  placeholder="Your Full Name"
                />
                <User className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" size={18} />
              </div>
            </div>

            <div>
              <label htmlFor="phoneNumber" className="block text-sm font-medium text-gray-700 mb-1">Phone Number</label>
              <div className="relative">
                <input
                  type="number"
                  id="phoneNumber"
                  name="phoneNumber"
                  value={formData.phoneNumber}
                  onChange={handleChange}
                  required
                  className="mt-1 block w-full pl-10 pr-3 py-2 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-purple-500 focus:border-purple-500 sm:text-sm transition duration-150 ease-in-out"
                  placeholder="e.g., 9876543210"
                />
                <Phone className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" size={18} />
              </div>
            </div>

            <div>
              <label htmlFor="email" className="block text-sm font-medium text-gray-700 mb-1">Email ID</label>
              <div className="relative">
                <input
                  type="email"
                  id="email"
                  name="email"
                  value={formData.email}
                  onChange={handleChange}
                  required
                  className="mt-1 block w-full pl-10 pr-3 py-2 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-purple-500 focus:border-purple-500 sm:text-sm transition duration-150 ease-in-out"
                  placeholder="you@example.com"
                />
                <Mail className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" size={18} />
              </div>
            </div>

            <div>
              <label htmlFor="collegeDepartment" className="block text-sm font-medium text-gray-700 mb-1">College/Department</label>
              <div className="relative">
                <input
                  type="text"
                  id="collegeDepartment"
                  name="collegeDepartment"
                  value={formData.collegeDepartment}
                  onChange={handleChange}
                  required
                  className="mt-1 block w-full pl-10 pr-3 py-2 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-purple-500 focus:border-purple-500 sm:text-sm transition duration-150 ease-in-out"
                  placeholder="e.g., University of Tech, CSE Dept."
                />
                <Building className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" size={18} />
              </div>
            </div>
          </div>

          {/* WhatsApp Sharing Button with Counter */}
          <div className="space-y-4 pt-4 border-t border-gray-200">
            <h2 className="text-xl font-semibold text-gray-800 flex items-center mb-4">
              <Share2 className="mr-2 text-green-500" size={20} /> Share & Support
            </h2>
            <button
              type="button"
              onClick={handleWhatsAppShare}
              disabled={whatsappClicks >= 5}
              className={`w-full flex items-center justify-center px-6 py-3 border border-transparent text-base font-medium rounded-lg shadow-sm ${
                whatsappClicks < 5
                  ? 'bg-green-500 hover:bg-green-600 text-white focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500'
                  : 'bg-gray-400 text-gray-700 cursor-not-allowed'
              } transition duration-150 ease-in-out transform hover:-translate-y-0.5`}
            >
              <svg className="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                <path d="M.057 24l1.687-6.163c-1.041-1.804-1.588-3.849-1.587-5.911.003-6.556 5.338-11.891 11.893-11.891 3.181.001 6.167 1.24 8.408 3.481 2.241 2.24 3.481 5.226 3.481 8.407 0 6.556-5.336 11.891-11.893 11.891-.001 0-.001 0-.002 0h-3.925l-1.163 1.163-1.163 1.163zm6.593-4.809l-.328-.198c-.524-.316-1.18-.46-1.849-.459-1.905-.001-3.458-1.554-3.458-3.459 0-1.905 1.553-3.458 3.458-3.458 1.905 0 3.458 1.553 3.458 3.458 0 1.905-1.553 3.458-3.458 3.458zm5.253 2.923c.316.001.632-.001.948-.001 4.542-.001 8.232-3.691 8.232-8.232 0-4.542-3.69-8.232-8.232-8.232-4.542 0-8.232 3.69-8.232 8.232 0 4.542 3.69 8.232 8.232 8.232z"/>
              </svg>
              Share on WhatsApp
            </button>
            <p className="text-center text-sm text-gray-600 mt-2">
              {whatsappMessage}
            </p>
          </div>

          {/* Screenshot Upload */}
          <div className="space-y-4 pt-4 border-t border-gray-200">
            <h2 className="text-xl font-semibold text-gray-800 flex items-center mb-4">
              <UploadCloud className="mr-2 text-blue-500" size={20} /> Upload Screenshot
            </h2>
            <label htmlFor="screenshotUpload" className="block text-sm font-medium text-gray-700 mb-1">Upload your resume, photo, etc.</label>
            <input
              type="file"
              id="screenshotUpload"
              name="screenshot"
              accept="image/*, application/pdf" // Accept images and PDFs
              onChange={handleFileChange}
              className="block w-full text-sm text-gray-500
                file:mr-4 file:py-2 file:px-4
                file:rounded-full file:border-0
                file:text-sm file:font-semibold
                file:bg-purple-50 file:text-purple-700
                hover:file:bg-purple-100 transition duration-150 ease-in-out"
            />
            {selectedFile && (
              <p className="text-sm text-gray-500 mt-2">Selected file: <span className="font-medium text-gray-700">{selectedFile.name}</span></p>
            )}
          </div>

          {/* Submit Registration Button */}
          <div className="pt-4 border-t border-gray-200">
            <button
              type="submit"
              className="w-full flex items-center justify-center px-6 py-3 border border-transparent text-base font-medium rounded-lg shadow-sm text-white bg-purple-600 hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-purple-500 transition duration-150 ease-in-out transform hover:-translate-y-0.5"
            >
              <Send className="mr-2" size={20} /> Submit Registration
            </button>
            {submissionMessage && (
              <p className="mt-4 text-center text-sm font-medium text-green-700 bg-green-50 p-3 rounded-lg shadow-md">
                {submissionMessage}
              </p>
            )}
          </div>
        </form>
      </div>
    </div>
  );
};

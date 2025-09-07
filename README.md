
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Laughter</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            background-color: #121212;
            font-family: 'Inter', sans-serif;
            color: #E0E0E0;
        }
        /* Custom scrollbar for better aesthetics */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-thumb {
            background-color: #4a4a4a;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background-color: #5d5d5d;
        }
        ::-webkit-scrollbar-track {
            background-color: #2c2c2c;
        }

        .video-thumbnail {
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .video-thumbnail:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px rgba(0, 0, 0, 0.3);
        }
        .loading-spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #ff5722;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        .comment-item {
            animation: fadeIn 0.5s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .nav-button.active {
            @apply ring-2 ring-offset-2 ring-orange-500 ring-offset-gray-900;
        }
    </style>
</head>
<body>

    <!-- Main Container -->
    <div class="min-h-screen flex flex-col items-center">

        <!-- Top Navigation Bar -->
        <header class="w-full bg-gray-900 p-4 shadow-md flex justify-between items-center z-10 sticky top-0">
            <div class="flex items-center space-x-6">
                <h1 id="home-link" class="text-3xl font-bold text-orange-500 cursor-pointer rounded-lg px-2 py-1 transition-colors duration-200 hover:bg-gray-800">Laughter</h1>
                <div class="flex items-center space-x-2">
                    <button id="home-btn" class="nav-button bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-6 rounded-full transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-orange-500">Home</button>
                    <button id="my-uploads-btn" class="nav-button bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-6 rounded-full transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-orange-500">My Uploads</button>
                    <button id="upload-btn" class="nav-button flex items-center bg-blue-500 hover:bg-blue-600 text-white font-semibold py-2 px-6 rounded-full transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
                            <path fill-rule="evenodd" d="M3 17a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM6.293 6.707a1 1 0 010-1.414l3-3a1 1 0 011.414 0l3 3a1 1 0 01-1.414 1.414L11 5.414V13a1 1 0 11-2 0V5.414L6.707 6.707a1 1 0 01-1.414 0z" clip-rule="evenodd" />
                        </svg>
                        Upload
                    </button>
                </div>
            </div>
            <div class="flex items-center space-x-4">
                <button id="profile-btn" class="nav-button bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-6 rounded-full transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-orange-500">My Profile</button>
                <div id="auth-status" class="text-xs text-gray-400">Loading user...</div>
                <div class="relative w-72">
                    <input type="text" id="search-input" placeholder="Search for comedy videos..." class="w-full px-4 py-2 pl-10 rounded-full bg-gray-800 text-gray-200 focus:outline-none focus:ring-2 focus:ring-orange-500 transition-all duration-200">
                    <svg class="w-5 h-5 absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" fill="currentColor" viewBox="0 0 20 20">
                        <path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clip-rule="evenodd" />
                    </svg>
                </div>
                <button id="search-button" class="bg-orange-500 hover:bg-orange-600 text-white font-semibold py-2 px-6 rounded-full transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-orange-500">Search</button>
            </div>
        </header>

        <!-- Main Content Area -->
        <main id="app-content" class="container mx-auto p-6 flex-grow">
            <!-- Content will be dynamically injected here -->
        </main>

    </div>

    <!-- Custom Modal for Alerts -->
    <div id="alert-modal" class="fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center hidden z-50">
        <div class="bg-gray-800 p-6 rounded-lg shadow-xl max-w-sm w-full border border-gray-700">
            <h3 class="text-xl font-bold text-orange-500 mb-4">Alert</h3>
            <p id="alert-message" class="text-gray-300"></p>
            <div class="mt-6 flex justify-end">
                <button id="ok-alert-btn" class="bg-orange-500 hover:bg-orange-600 text-white font-semibold py-2 px-6 rounded-lg transition-colors duration-200">OK</button>
            </div>
        </div>
    </div>

    <!-- Result Modal for AI-Generated Content -->
    <div id="result-modal" class="fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center hidden z-50">
        <div class="bg-gray-800 p-6 rounded-lg shadow-xl max-w-lg w-full border border-gray-700">
            <h3 id="result-title" class="text-xl font-bold text-orange-500 mb-4"></h3>
            <div class="p-4 bg-gray-900 rounded-lg text-gray-300 border border-gray-700 space-y-4">
                <p id="result-content" class="whitespace-pre-wrap"></p>
                <div class="flex justify-end">
                    <button id="copy-btn" class="bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-gray-500">
                        Copy to Clipboard
                    </button>
                </div>
            </div>
            <div class="mt-6 flex justify-end">
                <button id="close-result-btn" class="bg-orange-500 hover:bg-orange-600 text-white font-semibold py-2 px-6 rounded-lg transition-colors duration-200">Close</button>
            </div>
        </div>
    </div>

    <!-- User Profile Modal -->
    <div id="profile-modal" class="fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center hidden z-50">
        <div class="bg-gray-800 p-6 rounded-lg shadow-xl max-w-lg w-full border border-gray-700">
            <h3 class="text-xl font-bold text-orange-500 mb-4">My Account Details</h3>
            <div class="p-4 bg-gray-900 rounded-lg text-gray-300 border border-gray-700 space-y-4">
                <p class="text-sm font-semibold text-orange-400">Your unique ID:</p>
                <code id="profile-user-id" class="block bg-gray-700 p-2 rounded-lg text-sm text-gray-200 break-all"></code>
                <p class="text-xs text-gray-400 mt-2">This ID is your permanent, secure account for this app. All your uploads are linked to it.</p>
            </div>
            <div class="mt-6 flex justify-end">
                <button id="close-profile-btn" class="bg-orange-500 hover:bg-orange-600 text-white font-semibold py-2 px-6 rounded-lg transition-colors duration-200">Close</button>
            </div>
        </div>
    </div>

    <!-- Loading Overlay -->
    <div id="loading-overlay" class="fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center hidden z-50">
        <div class="loading-spinner"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, onSnapshot, query, addDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // --- Firebase Setup & State ---
        let app, db, auth;
        let userId = null;
        let isAuthReady = false;

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');

        // --- UI Elements ---
        const appContent = document.getElementById('app-content');
        const searchInput = document.getElementById('search-input');
        const searchButton = document.getElementById('search-button');
        const homeLink = document.getElementById('home-link');
        const homeBtn = document.getElementById('home-btn');
        const uploadBtn = document.getElementById('upload-btn');
        const myUploadsBtn = document.getElementById('my-uploads-btn');
        const profileBtn = document.getElementById('profile-btn');
        const authStatusEl = document.getElementById('auth-status');
        const loadingOverlay = document.getElementById('loading-overlay');
        const navButtons = document.querySelectorAll('.nav-button');
        const alertModal = document.getElementById('alert-modal');
        const okAlertBtn = document.getElementById('ok-alert-btn');
        const resultModal = document.getElementById('result-modal');
        const resultTitle = document.getElementById('result-title');
        const resultContent = document.getElementById('result-content');
        const closeResultBtn = document.getElementById('close-result-btn');
        const copyBtn = document.getElementById('copy-btn');
        const profileModal = document.getElementById('profile-modal');
        const profileUserIdEl = document.getElementById('profile-user-id');
        const closeProfileBtn = document.getElementById('close-profile-btn');

        // --- API & Utility Functions ---

        /** Sets the active state for a navigation button. */
        function setActiveNav(activeBtn) {
            navButtons.forEach(btn => btn.classList.remove('active'));
            if (activeBtn) {
                activeBtn.classList.add('active');
            }
        }

        /** Updates the document title dynamically. */
        function updatePageTitle(pageName) {
            document.title = `Laughter - ${pageName}`;
        }

        function customAlert(message) {
            document.getElementById('alert-message').textContent = message;
            alertModal.classList.remove('hidden');
        }

        function hideAlert() {
            alertModal.classList.add('hidden');
        }

        function showResult(title, content) {
            resultTitle.textContent = title;
            resultContent.textContent = content;
            copyBtn.onclick = () => copyToClipboard(content);
            resultModal.classList.remove('hidden');
        }

        function hideResult() {
            resultModal.classList.add('hidden');
        }

        function showProfileModal() {
            if (userId) {
                profileUserIdEl.textContent = userId;
                profileModal.classList.remove('hidden');
            } else {
                customAlert("User ID not available yet. Please wait for authentication.");
            }
        }

        function hideProfileModal() {
            profileModal.classList.add('hidden');
        }

        function copyToClipboard(text) {
            const textarea = document.createElement('textarea');
            textarea.value = text;
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand('copy');
            document.body.removeChild(textarea);
            customAlert("Copied to clipboard!");
        }

        function showLoading() {
            loadingOverlay.classList.remove('hidden');
        }

        function hideLoading() {
            loadingOverlay.classList.add('hidden');
        }

        /**
         * Uses the Google Search tool to find comedy videos on YouTube.
         * @param {string} query The search query.
         * @returns {Promise<Array>} A promise that resolves to an array of video data objects.
         */
        async function searchVideos(query) {
            showLoading();
            const systemPrompt = "Find and list a few YouTube comedy videos. For each video, provide the title, channel name, and a short, funny description. If available, also provide the YouTube video ID from the URL and the full URL.";
            const userQuery = `Find comedy videos on YouTube related to "${query}". Focus on providing the video ID, full URL, title, channel name, and a funny description.`;
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                tools: [{ "google_search": {} }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
                generationConfig: {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "ARRAY",
                        items: {
                            type: "OBJECT",
                            properties: {
                                "id": { "type": "STRING" },
                                "title": { "type": "STRING" },
                                "channel": { "type": "STRING" },
                                "description": { "type": "STRING" },
                                "url": { "type": "STRING" }
                            }
                        }
                    }
                }
            };

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                
                const candidate = result.candidates?.[0];
                if (candidate && candidate.content?.parts?.[0]?.text) {
                    const jsonString = candidate.content.parts[0].text;
                    console.log("Raw JSON response:", jsonString);
                    try {
                        const parsedData = JSON.parse(jsonString);
                        const filteredData = parsedData.filter(video => video.id);
                        return filteredData;
                    } catch (parseError) {
                        console.error("Error parsing JSON from search results:", parseError, "Raw data:", jsonString);
                        return [];
                    }
                } else {
                    console.error("API response structure is invalid:", result);
                    return [];
                }
            } catch (error) {
                console.error("Error fetching search results:", error);
                customAlert("Failed to fetch search results. Please try again.");
                return [];
            } finally {
                hideLoading();
            }
        }

        /**
         * Uses the LLM to generate a list of humorous comments for a video.
         * @param {string} videoTitle The title of the video to generate comments for.
         * @returns {Promise<Array<string>>} A promise that resolves to an array of comment strings.
         */
        async function generateComments(videoTitle) {
            const systemPrompt = "You are a witty commenter for a YouTube video. Your comments are short, humorous, and relatable. Use internet slang and emojis. Respond with a JSON array of 5 comment strings.";
            const userPrompt = `Generate comments for a video titled "${videoTitle}".`;
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

            const payload = {
                contents: [{ parts: [{ text: userPrompt }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
                generationConfig: {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "ARRAY",
                        items: { "type": "STRING" }
                    }
                }
            };
            
            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                
                const candidate = result.candidates?.[0];
                if (candidate && candidate.content?.parts?.[0]?.text) {
                    const jsonString = candidate.content.parts[0].text;
                    console.log("Raw JSON comments response:", jsonString);
                    try {
                        return JSON.parse(jsonString);
                    } catch (parseError) {
                        console.error("Error parsing JSON from comments:", parseError, "Raw data:", jsonString);
                        return ["This video is hilarious! 😂", "Can't stop laughing!", "I needed this today.", "This is gold!"];
                    }
                } else {
                    console.error("API response structure is invalid:", result);
                    return ["This video is hilarious! 😂", "Can't stop laughing!", "I needed this today.", "This is gold!"];
                }
            } catch (error) {
                console.error("Error generating comments:", error);
                return ["This video is hilarious! 😂", "Can't stop laughing!", "I needed this today.", "This is gold!"];
            }
        }
        
        /**
         * Uses the LLM to generate a short, witty joke based on the video title.
         * @param {string} videoTitle The title of the video.
         * @returns {Promise<string>} A promise that resolves to the joke text.
         */
        async function generateJoke(videoTitle) {
            const systemPrompt = "You are a professional comedian. You write a single, short, witty joke based on a given topic. Avoid being rude or offensive.";
            const userPrompt = `Write a short joke related to the video title: "${videoTitle}".`;
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
        
            const payload = {
                contents: [{ parts: [{ text: userPrompt }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
            };
        
            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                return result.candidates?.[0]?.content?.parts?.[0]?.text || "Failed to generate joke.";
            } catch (error) {
                console.error("Error generating joke:", error);
                return "Failed to generate joke. Please try again.";
            }
        }

        /**
         * Uses the LLM and Google Search to generate a video summary.
         * @param {string} videoTitle The video title.
         * @param {string} videoUrl The video URL.
         * @returns {Promise<string>} A promise that resolves to the summary text.
         */
        async function generateSummary(videoTitle, videoUrl) {
            const systemPrompt = "You are a concise video summarizer. Provide a brief, single-paragraph summary of the key content and humorous points of a YouTube video based on its title and URL.";
            const userQuery = `Summarize the content of the YouTube video titled "${videoTitle}" at this URL: ${videoUrl}.`;
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                tools: [{ "google_search": {} }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
            };

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                return result.candidates?.[0]?.content?.parts?.[0]?.text || "Failed to generate summary.";
            } catch (error) {
                console.error("Error generating summary:", error);
                return "Failed to generate summary. Please try again.";
            }
        }

        /**
         * Uses the LLM to generate a funny social media post about a video.
         * @param {string} videoTitle The video title.
         * @param {string} videoUrl The video URL.
         * @returns {Promise<string>} A promise that resolves to the social media post text.
         */
        async function generateSocialPost(videoTitle, videoUrl) {
            const systemPrompt = "You are a creative social media manager for a comedy channel. Write a short, engaging, and humorous social media post about a video. The post should be in a casual, relatable tone and include a link to the video.";
            const userQuery = `Write a funny social media post for a video titled "${videoTitle}". The link is: ${videoUrl}. Keep the post short and witty, suitable for platforms like Twitter.`;
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
            };
            
            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                return result.candidates?.[0]?.content?.parts?.[0]?.text || "Failed to generate social media post.";
            } catch (error) {
                console.error("Error generating social media post:", error);
                return "Failed to generate social media post. Please try again.";
            }
        }
        
        // --- UI Rendering Functions ---

        function createVideoCard(video, isUploaded = false) {
            const videoCard = document.createElement('div');
            videoCard.className = 'video-thumbnail bg-gray-800 rounded-xl overflow-hidden shadow-lg border border-gray-700';
            videoCard.onclick = () => renderWatchPage(video);
            
            const thumbnailUrl = `https://placehold.co/1280x720/1a202c/ffffff?text=${isUploaded ? 'Your Video' : 'Video Thumbnail'}`;

            videoCard.innerHTML = `
                <div class="relative w-full aspect-video">
                    <img src="${thumbnailUrl}" alt="${video.title} thumbnail" class="w-full h-full object-cover">
                    <p class="absolute bottom-2 right-2 bg-black bg-opacity-75 text-white text-xs px-2 py-1 rounded-md">1:30</p>
                </div>
                <div class="p-4">
                    <h3 class="text-lg font-semibold truncate text-orange-400">${video.title}</h3>
                    <p class="text-sm text-gray-400">${video.channel}</p>
                    <p class="text-xs text-gray-500 mt-2 line-clamp-2">${video.description}</p>
                </div>
            `;
            return videoCard;
        }

        function renderHomePage() {
            if (!isAuthReady) return;
            appContent.innerHTML = '';
            showLoading();
            updatePageTitle('Home');
            setActiveNav(homeBtn);

            const publicVideosRef = collection(db, `artifacts/${appId}/public/data/videos`);
            const q = query(publicVideosRef);

            onSnapshot(q, (snapshot) => {
                const videos = [];
                snapshot.forEach((doc) => {
                    videos.push({ id: doc.id, ...doc.data() });
                });
                
                const gridContainer = document.createElement('div');
                gridContainer.className = 'grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 animate-fadeIn';
                
                videos.forEach(video => {
                    gridContainer.appendChild(createVideoCard(video));
                });
                
                appContent.innerHTML = '';
                appContent.appendChild(gridContainer);
                hideLoading();
            });
        }
        
        function renderMyUploadsPage() {
            if (!isAuthReady || !userId) {
                customAlert("Please wait for authentication to load.");
                return;
            }
            appContent.innerHTML = '';
            showLoading();
            updatePageTitle('My Uploads');
            setActiveNav(myUploadsBtn);
            
            const userVideosRef = collection(db, `artifacts/${appId}/users/${userId}/my_uploads`);
            const q = query(userVideosRef);

            onSnapshot(q, (snapshot) => {
                const userVideos = [];
                snapshot.forEach((doc) => {
                    userVideos.push({ docId: doc.id, ...doc.data() });
                });
                
                appContent.innerHTML = '';
                if (userVideos.length === 0) {
                    appContent.innerHTML = `<p class="text-center text-gray-400 text-lg mt-10">You have not uploaded any videos yet. Go to the Upload page to add your first video!</p>`;
                } else {
                    const gridContainer = document.createElement('div');
                    gridContainer.className = 'grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 animate-fadeIn';
                    userVideos.forEach(video => {
                        gridContainer.appendChild(createVideoCard(video, true));
                    });
                    appContent.appendChild(gridContainer);
                }
                hideLoading();
            });
        }

        function renderUploadPage() {
            if (!isAuthReady || !userId) {
                customAlert("Please wait for authentication to load.");
                return;
            }
            appContent.innerHTML = '';
            updatePageTitle('Upload Video');
            setActiveNav(uploadBtn);

            const uploadFormHtml = `
                <div class="bg-gray-800 p-8 rounded-xl shadow-lg border border-gray-700 max-w-lg mx-auto">
                    <h2 class="text-2xl font-bold text-orange-400 mb-6">Add a Video</h2>
                    <div class="space-y-4">
                        <button id="camera-btn" class="w-full flex items-center justify-center bg-gray-700 hover:bg-gray-600 text-white font-semibold py-3 px-6 rounded-lg transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-gray-500">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.863-1.29a2 2 0 011.664-.89h1.84c.732 0 1.442.368 1.664.89l.863 1.29a2 2 0 001.664.89H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2v-9z" />
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" />
                            </svg>
                            Record & Upload
                        </button>
                    </div>
                    <div class="my-6 flex items-center">
                        <div class="flex-grow border-t border-gray-600"></div>
                        <span class="flex-shrink mx-4 text-gray-500">OR</span>
                        <div class="flex-grow border-t border-gray-600"></div>
                    </div>
                    <form id="upload-form" class="space-y-4">
                        <div>
                            <label for="video-id" class="block text-sm font-medium text-gray-400">YouTube Video ID or Full URL</label>
                            <input type="text" id="video-id" placeholder="e.g., dQw4w9WgXcQ or full URL" class="mt-1 block w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-gray-200 focus:outline-none focus:ring-2 focus:ring-orange-500">
                            <p class="text-xs text-gray-500 mt-1">Paste a full YouTube URL and the ID will be extracted automatically!</p>
                        </div>
                        <div>
                            <label for="video-title" class="block text-sm font-medium text-gray-400">Video Title</label>
                            <input type="text" id="video-title" placeholder="A funny video title" class="mt-1 block w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-gray-200 focus:outline-none focus:ring-2 focus:ring-orange-500">
                        </div>
                        <div>
                            <label for="video-desc" class="block text-sm font-medium text-gray-400">Description</label>
                            <textarea id="video-desc" rows="3" placeholder="A short, funny description" class="mt-1 block w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-gray-200 focus:outline-none focus:ring-2 focus:ring-orange-500"></textarea>
                        </div>
                        <button type="submit" class="w-full bg-blue-500 hover:bg-blue-600 text-white font-semibold py-2 px-6 rounded-lg transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-blue-500">
                            Add YouTube Video
                        </button>
                    </form>
                </div>
            `;
            appContent.innerHTML = uploadFormHtml;

            const cameraBtn = document.getElementById('camera-btn');
            cameraBtn.addEventListener('click', () => {
                customAlert("To add a video, please paste a YouTube URL into the input field below. This app does not support direct video uploads from your device.");
            });

            // New logic to extract video ID from a URL
            const videoIdInput = document.getElementById('video-id');
            videoIdInput.addEventListener('input', (event) => {
                const url = event.target.value;
                const urlPattern = /(?:https?:\/\/)?(?:www\.)?(?:youtube\.com\/(?:watch\?v=|embed\/)|youtu\.be\/)([a-zA-Z0-9_-]{11})/;
                const match = url.match(urlPattern);
                if (match && match[1]) {
                    videoIdInput.value = match[1];
                }
            });
            
            document.getElementById('upload-form').addEventListener('submit', async (e) => {
                e.preventDefault();
                showLoading();
                const videoId = document.getElementById('video-id').value.trim();
                const videoTitle = document.getElementById('video-title').value.trim() || 'Untitled Video';
                const videoDesc = document.getElementById('video-desc').value.trim() || 'No description provided.';
                const channel = userId;

                if (!videoId) {
                    customAlert("Please enter a YouTube Video ID.");
                    hideLoading();
                    return;
                }

                try {
                    const userVideosRef = collection(db, `artifacts/${appId}/users/${userId}/my_uploads`);
                    await addDoc(userVideosRef, {
                        id: videoId,
                        title: videoTitle,
                        description: videoDesc,
                        channel: channel,
                        createdAt: serverTimestamp()
                    });
                    customAlert("Video added successfully!");
                    renderMyUploadsPage();
                } catch (error) {
                    console.error("Error adding document: ", error);
                    customAlert("Failed to add video. Please try again.");
                } finally {
                    hideLoading();
                }
            });
        }
        
        async function renderWatchPage(video) {
            appContent.innerHTML = '';
            updatePageTitle(video.title);
            setActiveNav(null); // No nav button is active on the watch page
            
            const channelName = video.channel === userId ? "My Upload" : video.channel;

            const videoPlayerSection = document.createElement('div');
            videoPlayerSection.className = 'w-full max-w-5xl mx-auto space-y-4';
            videoPlayerSection.innerHTML = `
                <div class="w-full aspect-video bg-black rounded-xl overflow-hidden">
                    <iframe 
                        id="youtube-player"
                        src="https://www.youtube.com/embed/${video.id}?autoplay=1"
                        frameborder="0"
                        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                        allowfullscreen
                        class="w-full h-full"
                    ></iframe>
                </div>
                <div class="bg-gray-800 p-6 rounded-xl shadow-lg border border-gray-700">
                    <h2 class="text-3xl font-bold text-orange-400">${video.title}</h2>
                    <p class="text-gray-400 mt-1">${channelName}</p>
                    <div class="mt-4 text-sm text-gray-300 space-y-2">
                        <p class="font-semibold">Description:</p>
                        <p>${video.description}</p>
                    </div>
                </div>
            `;
            appContent.appendChild(videoPlayerSection);

            const commentsSection = document.createElement('div');
            commentsSection.className = 'w-full max-w-5xl mx-auto mt-8';
            commentsSection.innerHTML = `
                <div class="flex items-center justify-between mb-4 flex-wrap gap-2">
                    <h3 class="text-xl font-bold text-gray-200">Comments</h3>
                    <div class="flex items-center gap-2">
                        <button id="summarize-btn" class="bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-4 rounded-full transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-gray-500">
                            ✨ Summarize
                        </button>
                        <button id="social-post-btn" class="bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-4 rounded-full transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-gray-500">
                            ✨ Share on Social
                        </button>
                        <button id="generate-joke-btn" class="bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-4 rounded-full transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-gray-500">
                            ✨ Generate Joke
                        </button>
                        <button id="generate-comments-btn" class="bg-orange-500 hover:bg-orange-600 text-white font-semibold py-2 px-4 rounded-full transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-orange-500">
                            Generate More Comments
                        </button>
                    </div>
                </div>
                <div id="comment-list" class="space-y-4">
                    <div class="text-center text-gray-400">Loading comments...</div>
                </div>
            `;
            appContent.appendChild(commentsSection);

            const commentList = document.getElementById('comment-list');
            const comments = await generateComments(video.title);
            commentList.innerHTML = '';
            if (comments.length > 0) {
                comments.forEach(commentText => {
                    const commentDiv = document.createElement('div');
                    commentDiv.className = 'comment-item bg-gray-800 p-4 rounded-lg shadow border border-gray-700 text-gray-300';
                    commentDiv.textContent = commentText;
                    commentList.appendChild(commentDiv);
                });
            } else {
                commentList.innerHTML = '<div class="text-center text-gray-400">No comments to display.</div>';
            }

            document.getElementById('generate-comments-btn').onclick = async () => {
                commentList.innerHTML = '<div class="text-center text-gray-400">Generating new comments...</div>';
                const newComments = await generateComments(video.title);
                commentList.innerHTML = '';
                newComments.forEach(commentText => {
                    const commentDiv = document.createElement('div');
                    commentDiv.className = 'comment-item bg-gray-800 p-4 rounded-lg shadow border border-gray-700 text-gray-300';
                    commentDiv.textContent = commentText;
                    commentList.appendChild(commentDiv);
                });
            };

            document.getElementById('summarize-btn').onclick = async () => {
                showLoading();
                const summary = await generateSummary(video.title, video.url);
                hideLoading();
                showResult('Video Summary', summary);
            };

            document.getElementById('social-post-btn').onclick = async () => {
                showLoading();
                const socialPost = await generateSocialPost(video.title, video.url);
                hideLoading();
                showResult('Social Media Post', socialPost);
            };
            
            document.getElementById('generate-joke-btn').onclick = async () => {
                showLoading();
                const joke = await generateJoke(video.title);
                hideLoading();
                showResult('AI-Generated Joke', joke);
            };
        }

        function renderSearchPage(videos, query) {
            appContent.innerHTML = '';
            updatePageTitle(`Search: ${query}`);
            setActiveNav(null); // No nav button is active on search results page

            const header = document.createElement('h2');
            header.className = 'text-2xl font-bold text-orange-400 mb-6';
            header.textContent = `Search results for "${query}"`;
            appContent.appendChild(header);

            const gridContainer = document.createElement('div');
            gridContainer.className = 'grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 animate-fadeIn';
            videos.forEach(video => {
                gridContainer.appendChild(createVideoCard(video));
            });
            appContent.appendChild(gridContainer);
        }

        // --- Event Listeners and Initial Load ---
        
        homeLink.addEventListener('click', () => {
            renderHomePage();
            searchInput.value = '';
        });
        
        homeBtn.addEventListener('click', () => {
            renderHomePage();
            searchInput.value = '';
        });

        myUploadsBtn.addEventListener('click', () => {
            renderMyUploadsPage();
            searchInput.value = '';
        });

        uploadBtn.addEventListener('click', () => {
            renderUploadPage();
            searchInput.value = '';
        });

        profileBtn.addEventListener('click', showProfileModal);
        
        // Fix: Added event listeners for modal buttons to avoid ReferenceError
        okAlertBtn.addEventListener('click', hideAlert);
        closeResultBtn.addEventListener('click', hideResult);
        closeProfileBtn.addEventListener('click', hideProfileModal);


        searchButton.addEventListener('click', async () => {
            const query = searchInput.value.trim();
            if (query) {
                const searchResults = await searchVideos(query);
                if (searchResults.length > 0) {
                    renderSearchPage(searchResults, query);
                } else {
                    appContent.innerHTML = `<p class="text-center text-gray-400 text-lg mt-10">No videos found for "${query}". Try a different search.</p>`;
                }
            } else {
                customAlert("Please enter a search query.");
            }
        });

        window.onload = function() {
            try {
                if (Object.keys(firebaseConfig).length === 0) {
                     throw new Error("Firebase config not available. Cannot initialize app.");
                }
                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);
                
                onAuthStateChanged(auth, async (user) => {
                    if (user) {
                        userId = user.uid;
                        isAuthReady = true;
                        authStatusEl.textContent = `User ID: ${userId.substring(0, 8)}...`;
                        renderHomePage();
                    } else {
                        try {
                            const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
                            if (initialAuthToken) {
                                await signInWithCustomToken(auth, initialAuthToken);
                            } else {
                                await signInAnonymously(auth);
                            }
                        } catch (error) {
                            console.error("Authentication failed:", error);
                            authStatusEl.textContent = "Authentication failed.";
                        }
                    }
                });
            } catch (e) {
                console.error("Failed to initialize Firebase:", e);
                authStatusEl.textContent = "Error: Firebase not initialized.";
                appContent.innerHTML = `<p class="text-center text-red-500 mt-10">Failed to initialize the app. Check console for details.</p>`;
            }
        };
    </script>
</body>
</html>

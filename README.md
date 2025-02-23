# LANxfer

LANxfer is a simple, secure file transfer application designed for local area networks (LANs). It allows users to upload encrypted files, share them with specific devices or everyone on the network, and download them with automatic decryption—all within a cyberpunk-themed web interface. Built with Flask (Python) and JavaScript, it uses AES-256 encryption to ensure file security.

## Features
- *File Upload*: Drag and drop or select files to upload, encrypted with AES-256.
- *Recipient Selection*: Share files with "Everyone" or a specific IP address on the LAN.
- *IP Tracking*: Dynamically tracks active devices on the network with a timeout mechanism (IPs are removed after 30 seconds of inactivity).
- *File Listing*: Displays available files with sorting options (name, size, modified date).
- *Download & Decrypt*: Downloads files and decrypts them client-side using CryptoJS.
- *Cyberpunk UI*: A sleek, neon-green-on-black interface inspired by cyberpunk aesthetics.

## Prerequisites
- Python 3.6+
- Node.js (optional, for development convenience)
- A local network with devices to test file sharing

## Installation

1. *Clone the Repository*
   bash
   git clone https://github.com/yourusername/LANxfer.git
   cd LANxfer
   

2. *Install Python Dependencies*
   Install the required Python packages using pip:
   bash
   pip install Flask pycryptodome
   
   - Flask: Web framework for the server.
   - pycryptodome: Provides AES encryption functionality.

3. *Project Structure*
   Ensure your directory looks like this:

   
   LANxfer/
   ├── app.py
   ├── static/
   │   ├── script.js
   │   └── styles.css
   ├── templates/
   │   └── index.html
   ├── uploads/ (created automatically on first run)
   └── README.md
   

4. *Run the Server*
   Start the Flask application:
   bash
   python app.py
   
   The server will run on http://0.0.0.0:5000 (accessible from any device on your LAN).

## Usage

1. *Access the Web Interface*
   - Open a browser on any device in the same LAN and navigate to http://<server-ip>:5000 (replace <server-ip> with the IP of the machine running app.py, e.g., http://192.168.1.100:5000).

2. *Upload a File*
   - Drag a file into the drop zone or click "Choose File" to select one.
   - Select a recipient from the dropdown (default: "Everyone") or a specific IP.
   - Click "Upload" to encrypt and send the file to the server.

3. *Refresh IPs*
   - Click "Refresh IPs" next to the recipient dropdown to update the list of active devices. IPs disappear after 30 seconds of inactivity.

4. *View and Download Files*
   - The table below lists files you’re authorized to see (those addressed to your IP or "Everyone").
   - Click "Download" to retrieve and automatically decrypt a file.

## How It Works

### Server (app.py)
- *Encryption*: Files are encrypted with AES-256 in CBC mode using a pre-shared key (SECRET_KEY).
- *IP Management*: Tracks active IPs with timestamps, removing them after 30 seconds (TIMEOUT_MINUTES = 0.5) of inactivity via a background thread.
- *Endpoints*:
  - /: Serves the main page and tracks visitors.
  - /get_ips: Returns active IPs, updating the requester’s timestamp.
  - /get_files: Lists files for the requester, sorted by name, size, or date.
  - /upload: Handles file uploads and encryption.
  - /download/<filename>: Serves encrypted files with access control.

### Client (index.html, styles.css, script.js)
- *UI*: Cyberpunk-themed interface with drag-and-drop support and a sortable file table.
- *JavaScript*: Uses CryptoJS for client-side decryption, fetches IPs manually via a "Refresh IPs" button, and handles file uploads/downloads.
- *Timeout*: IPs are refreshed only on page load or button click, relying on the server’s timeout to remove inactive devices.

## Security Notes
- *Pre-shared Key*: The AES key (ThisIsASecretKey1234567890123456) is hardcoded in both app.py and script.js. In a production environment, distribute this securely and consider generating unique keys per session.
- *Encryption*: Files are encrypted server-side and decrypted client-side, ensuring data privacy over the LAN.
- *Access Control*: Downloads are restricted to the intended recipient IP or "Everyone."

## Configuration
- *Timeout*: Adjust TIMEOUT_MINUTES in app.py (currently 0.5 minutes = 30 seconds) to change how long IPs remain active without requests.
- *Port*: Modify app.run(port=5000) in app.py if you need a different port.

## Limitations
- *LAN Only*: Designed for local networks; not intended for internet use without additional security (e.g., HTTPS).
- *No Persistence*: File and IP data are stored in memory and lost on server restart. Add a database for persistence if needed.
- *No Authentication*: Relies on IP-based access control; no user login system.

## Troubleshooting
- *IPs Not Showing*: Ensure devices are on the same LAN and making requests (e.g., loading the page or clicking "Refresh IPs").
- *Timeout Too Fast*: If IPs disappear too quickly, increase TIMEOUT_MINUTES in app.py.
- *File Errors*: Check the server console for logs (e.g., file not found, access denied).

## Contributing
Feel free to fork this repository, submit issues, or send pull requests to enhance LANxfer!

## License
This project is open-source under the [MIT License](LICENSE).

# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 

### CLIENT
```
import requests

SERVER = "http://localhost:8000"

def upload_file(filename):
    with open(filename, 'rb') as f:
        files = {'file': (filename, f)}
        response = requests.post(f"{SERVER}/upload", files=files)
        print("Upload response:", response.text)

def download_file(filename):
    url = f"{SERVER}/download/{filename}"
    response = requests.get(url)
    if response.status_code == 200:
        with open(f"downloaded_{filename}", "wb") as f:
            f.write(response.content)
        print(f"Downloaded '{filename}' as 'downloaded_{filename}'.")
    else:
        print("Download failed:", response.status_code)

if __name__ == "__main__":
    # Example usage
    upload_file("example.txt")
    download_file("example.txt")
```
### SERVER
```
from flask import Flask, request, send_from_directory
import os

app = Flask(__name__)
UPLOAD_FOLDER = os.path.abspath('.')

@app.route('/upload', methods=['POST'])
def upload_file():
    file = request.files['file']
    filename = file.filename
    file.save(os.path.join(UPLOAD_FOLDER, filename))
    return f"'{filename}' uploaded successfully!", 200

@app.route('/download/<filename>', methods=['GET'])
def download_file(filename):
    return send_from_directory(UPLOAD_FOLDER, filename, as_attachment=True)

if __name__ == "__main__":
    app.run(port=8000)
```
## OUTPUT
### CLIENT
<img width="617" height="249" alt="image" src="https://github.com/user-attachments/assets/61bcd840-12e7-4074-9476-ef6bdbc1848a" />

### SERVER
<img width="847" height="287" alt="image" src="https://github.com/user-attachments/assets/13a29817-e73a-4366-ac14-d0e09ff40bcc" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed

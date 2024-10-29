# Eaters-Eye
Watch out for what you eat

Eater's eye is a full-stack AI web application. Returns the nutritional contents of the food captured through camera or uploaded via files.

Link to youtube video - https://youtu.be/fhysG7ToD-M?si=IdrfApYjC_pyZKtI


## Tech Stack

&nbsp;

This project uses:

Frontend: 
- Vue.js - https://vuejs.org/guide/introduction.html
- Tailwind CSS - https://tailwindcss.com/docs/installation
- HTML

Backend: 
- Python (Flask) - https://www.python.org/downloads/
- Google Cloud Vision API - https://cloud.google.com/vision/docs#docs
- CalorieNinja API - https://calorieninjas.com/api

Deployment:
- Vercel - https://vercel.com/docs


&nbsp;
## Backend

&nbsp;

### Install python virtual environment


https://flask.palletsprojects.com/en/3.0.x/installation/

Install python if not done. Then run the following below commands in your project directory.

Windows:
```sh
py -3 -m venv .venv
.venv\Scripts\activate
```

Linux:
```sh
sudo apt-get install python3-venv 
python3 -m venv .venv
source .venv/bin/activate
```

macOS:
```sh
python3 -m venv .venv
source .venv/bin/activate
```

### Install dependencies



If all the dependencies are already installed, skip this.
```sh
pip install -r requirements.txt
```

### Google vision api key


Generate your own API keys with Google Cloud Vision API. Enter all the credentials in 
File name: cred.json


### Run app

After your project has been created, run this. This is the command to start your final project
```sh
py -m flask --app BackendApi run
```

### URLs from local host

Your project will be run on this URL. 
```sh
http://127.0.0.1:5000/
```

&nbsp;

## Frontend

&nbsp;

Installing Vue 3. Make sure you have Node.js installed, (if not, check below link)

https://nodejs.org/en


Create node modules for your project
```sh
npm install
```

Compile and run server
```sh
npm run dev
```

Start production
```sh
npm build
```

&nbsp;

## Deployment

&nbsp;

Deploy the webapp on vercel. Login to vercel using github and deploy your github repo directly on vercel.

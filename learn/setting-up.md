## Setting up the Anuvaad Web Application

Follow these steps to set up the Anuvaad Web Application on your local machine:

### Frontend

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/project-anuvaad/anuvaad.git
 
2. **Navigate to the Project Directory:**
   ```bash
   cd anuvaad/anuvaad-fe/anuvaad-webapp-webapp
   ```

3. **Install Dependencies:**
   ```bash
   npm install
   ```
   or
   ```bash
   yarn install
   ```

4. **Environment Variables:**
   Create a `.env` file in the root directory of the project and configure the necessary environment variables. You can use the `.env.example` file as a reference.

5. **Start the Development Server:**
   ```bash
   npm start
   ```
   or
   ```bash
   yarn start
   ```

6. **Access the Application:**
   Once the development server is started, you can access the application by navigating to [http://localhost:3000](http://localhost:3000) in your web browser.

### Additional Commands

- **Build the Application:**
  ```bash
  npm run build
  ```
  or
  ```bash
  yarn build
  ```

- **Run Tests:**
  ```bash
  npm test
  ```
  or
  ```bash
  yarn test
  ```

### Backend

**General Guidelines:**

1. Clone the repo and go to the module specific directory.
2. Run `pip3 install -r requirements.txt`.
3. Make necessary changes to config files with respect to MongoDB and Kafka.
4. Run `python3 src/app.py`.

Alternatively, modules could be run by building and running Docker images. Make sure configs and ports are configured as per your local setup.

- **Build Docker Image:**
  ```bash
  docker build -t <service-name> .
  ```

- **Run Docker Container:**
  ```bash
  docker run -r <service-name>
  ```

**Note**: Apart from this, the Docker images running in the user's environment could be found [here](https://hub.docker.com/u/anuvaadio).
```

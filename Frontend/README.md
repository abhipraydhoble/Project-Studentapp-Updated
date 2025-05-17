### 3. Setup Frontend (React)

- Clone the project repository (if not already cloned):

```bash
git clone https://github.com/rajatpzade/cloudblitz-student-app.git
```

- Navigate to the frontend directory:

```bash
cd cloudblitz-student-app/frontend
```

- Install Node.js and npm:

```bash
sudo apt update
sudo apt install nodejs npm -y
```

- Verify installations:

```bash
node -v
npm -v
```

- Configure backend API URL:

Create or edit the `.env` file inside the `frontend/` directory and add your backend server IP:

```bash
REACT_APP_BACKEND_URL=http://<your-backend-server-ip>:8080
```

## other way 

### add backend IP into "src/api/userService.js"

```
import axios from "axios";

const BASE_URL = "http://54.91.185.15:8080/api";

export const fetchUsers = async () => {
  try {
    const response = await axios.get(`${BASE_URL}/users`);
    return response.data;
  } catch (error) {
    console.error("Error fetching users:", error);
    throw error;
  }
};

export const registerUser = async (userData) => {
  try {
    await axios.post(`${BASE_URL}/register`, userData);
  } catch (error) {
    console.error("Error registering user:", error);
    throw error;
  }
};

export const deleteUser = async (id) => {
  try {
    await axios.delete(`${BASE_URL}/users/${id}`);
  } catch (error) {
    console.error("Error deleting user:", error);
    throw error;
  }
};


```


- Install frontend dependencies:

```bash
npm install
```

- Start the frontend development server:

```bash
npm start
```

- Build the frontend production files:

```bash
npm run build
```

---

### 4. Host Frontend Using Apache2 (Optional)

- Install Apache2 server:

```bash
sudo apt install apache2 -y
```

- Copy the built files to Apache default web directory:

```bash
sudo cp -rv dist/* /var/www/html/
```

- Start Apache2 service:

```bash
sudo systemctl start apache2
```

- Now access your site using the public IP of your server.

**Alternatively**, you can host the React build folder using AWS S3 Static Website Hosting.

---

## 📂 Project Structure

```
cloudblitz-student-app/
├── backend/   # Spring Boot application
├── frontend/  # React application
```

---

## 👨‍💼 Author

- Rajat P. Zade

---

## 📢 Important Notes

- Make sure RDS Security Group allows access from backend server IP.
- Handle environment variables securely in production.
- Update frontend and backend URLs properly after deployment.

---
# Vue.js Form Task

The app is created using Vue.js and contains a dynamic form that validates input data, calculates gross price based on net price and VAT, and submits the form using AJAX.

## How to Install and Run the Application

### Development Version (Recommended for viewing the code)

1. Clone the repository to a folder:
    ```bash
    git clone https://github.com/PatrykW97/home-task/
    cd home-task
    ```

2. Install dependencies:
    ```bash
    npm install
    ```

3. Run the app in development mode:
    ```bash
    npm run dev
    ```

4. The app will be available at:
    ```
    http://localhost:5173
    ```

### Production Build

1. Build the app for production:
    ```bash
    npm run build
    ```

2. To preview the built application locally, run:
    ```bash
    npm run preview
    ```

### How the Application Works

1. The app is a dynamic form consisting of the following fields:
   - **Description**: Textarea with a maximum of 255 characters.
   - **Send confirmation**: Radio button (Yes/No).
   - **VAT**: Dropdown with VAT values (19%, 21%, 23%, 25%).
   - **Price netto EUR**: Text input that accepts only numeric values.
   - **Price brutto EUR**: This field is dynamically calculated based on the net price and VAT selection.

2. The form data is validated dynamically, and the form is submitted via AJAX to a simulated endpoint.

3. After successful form submission, the form is hidden, and a success message is displayed to the user.

### Requirements

- Node.js (version 20+)
- npm
- Code editor preferably Visual Studio Code.

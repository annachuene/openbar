Open Bar Web Application

This is a small web application built with Nuxt.js for managing an Open bar tap. Users are able to add/remove drinks, specify quantities, and split the bill among multiple people. The application allows exporting the info from tab as CSV or PDF for generating receipts.

##Features

Users can select from three types of beverages (Beer, Cider, Premix) and specify quantities to add to their open tab.
Split Bill: Optionally, users can specify how many people will split the total bill.
Current Tab Display: Shows the list of added beverages with quantities, total cost, and price per person (if splitting the bill).
Export Options: Export the tab as CSV or PDF for receipt generation.

##Technologies Used:
Vue.js - Frontend framework for building user interfaces.
Nuxt.js -  Vue.js framework for server-side rendering and routing.
file-saver - Library for saving files on the client-side.
jspdf - Library for generating PDF documents in JavaScript.

##Appication Setup
Clone the repository:

Open command line interface and run the command below
Copy code
git clone <repository-url>
cd openbar
Install dependencies:

Open command line interface and run the command below
Copy code
npm install
Run the application:

Open command line interface and run the command below
Copy code
npm run dev
Open your browser and navigate to http://localhost:3000 to view the application.

##Usage
Fill in the quantities of each beverage type you want to add and click "Add Beverage Button".
Optionally, enter the number of people sharing the bill.
Viewing Total and Exporting: See the beverage details including total cost and optionally price per person. Export the tab as CSV or PDF for receipts.





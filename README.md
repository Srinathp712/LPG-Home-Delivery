# LPG-Home-Delivery
This project was inspired with real time problem statement.
## Contents
### -> OBJECTIVE
### -> PROBLEM STATEMENT
### -> OVERVIEW
### -> CODE
### -> CONCLUSION

## OBJECTIVE

## PROBLEM STATEMENT


## OVERVIEW
### 1. My story begins when a customer opens the website at http://127.0.0.1:5000.
#### -At this point, the server (Waitress + Flask) starts running and loads the home page (/).
#### -The webpage shows a form with three input fields:
#### -Name, Phone Number, Address
#### -Below the form, there is a "Send OTP" button.
### 2. User Fills in Their Details and Clicks "Send OTP"
#### -EX: Name: John Doe
####     Phone: 9876543210
####     Address: 123 Main Street, New Delhi
#### -Now,they click the "Send OTP" button.
### 3. What happens afyer clicking "Send OTP"
#### -Since it's a POST request, Flask checks which button was clicked:
#### -Here, the "Send OTP" button was clicked, so the server knows the user wants an OTP.
#### -The Flask app now processes the request:
#### -It generates a random OTP (e.g., 4823).
#### -It stores this OTP in a dictionary (self.otp_store) so it can be used later.
#### -It creates a text file (customer_details/phone.txt) to store
#### -It displays a message on the webpage:
#### -"OTP Sent Successfully!"
#### -The page reloads with a second form to enter the OTP.
### 4. User Receives OTP and Enters It on the Page
#### -The browser sends another POST request to the server.
#### -Flask checks which button was clicked:
#### -Since "Verify OTP" was clicked, the server checks the entered OTP.
#### -The Flask app now verifies the OTP:
#### -It retrieves the OTP stored in self.otp_store[9876543210].
#### -It compares the entered OTP (4823) with the stored one (4823).
#### -If they match, Flask updates the page to say:
#### -"OTP Verified Successfully! Sit back and relax, we will deliver the LPG to your home."
#### -If it's wrong, Flask updates the page with:
#### -"Invalid OTP! Try again."
### 5. OTP Verified → LPG is Ready for Delivery!
#### -Once the OTP is verified:
#### -The system confirms the delivery request.
#### -The customer can see the confirmation message on the webpage.
#### -The order is stored safely in the server’s customer database (.txt files).
#### -The admin (gas company) can now process the LPG cylinder delivery.

### CODE

# LPG-Home-Delivery
This project was inspired with real time problem statement.

![LPG logo](https://github.com/Srinathp712/LPG-Home-Delivery/blob/main/image.png)
## Contents
### -> OBJECTIVE
### -> PROBLEM STATEMENT
### -> OVERVIEW
### -> CODE
### -> CONCLUSION

## OBJECTIVE
### Applicaion desinged to give LPG by home delivery service through internet. 

## PROBLEM STATEMENT
### One day my father was booking an LPG cylinder through an automated phone system (which required entering a gas booking ID, pressing multiple buttons like press 1 to confirm, 2 for cancel ect...), I noticed that the process was time-consuming and not user-friendly. To simplify this, I decided to develop an application where users can easily provide their details, and confirm their booking with OTP verification. The LPG company can then store this information to facilitate seamless home delivery.

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
#### PYTHON CODE
``` Python
from flask import Flask, request, render_template_string
import random
import os
from waitress import serve

class CustomerDelivery:
    def generate_otp(self, phone):
        raise NotImplementedError
    
    def store_customer_details(self, phone, name, address):
        raise NotImplementedError
    
    def verify_otp(self, phone, otp):
        raise NotImplementedError

class LPGDelivery(CustomerDelivery):
    def __init__(self):
        self.otp_store = {}
        self.customer_details_folder = "customer_details"
        os.makedirs(self.customer_details_folder, exist_ok=True)

    def generate_otp(self, phone):
        otp = random.randint(1000, 9999)
        self.otp_store[phone] = otp
        print(f"OTP for {phone}: {otp}")
        return otp

    def store_customer_details(self, phone, name, address):
        file_path = os.path.join(self.customer_details_folder, f"{phone}.txt")
        with open(file_path, "w") as f:
            f.write(f"Name: {name}\nPhone: {phone}\nAddress: {address}\n")
        return file_path

    def verify_otp(self, phone, otp):
        return self.otp_store.get(phone) == otp

app = Flask(__name__)
delivery_service = LPGDelivery()

@app.route('/', methods=['GET', 'POST'])
def home():
    otp_sent_message = ""
    verification_message = ""
    delivery_confirmation_message = ""
    
    # Use empty strings for inputs to avoid "None" in inputs
    name = phone = address = ""  

    if request.method == 'POST':
        # Check if it's the 'send OTP' or 'verify OTP' part of the form
        if 'send_otp' in request.form:
            # Retrieve form data
            name = request.form.get('name', "")
            phone = request.form.get('phone', "")
            address = request.form.get('address', "")
            
            # Generate OTP and store customer details
            if name and phone and address:
                otp = delivery_service.generate_otp(phone)
                delivery_service.store_customer_details(phone, name, address)
                otp_sent_message = "OTP Sent Successfully!"
            else:
                otp_sent_message = "Please fill in all the fields correctly."
        
        elif 'verify_otp' in request.form:
            # OTP verification
            otp = int(request.form.get('otp'))
            phone = request.form.get('phone')
            if delivery_service.verify_otp(phone, otp):
                verification_message = "OTP Verified Successfully!"
                delivery_confirmation_message = "OTP Verified! Sit back and relax, we will deliver the LPG to your home."
            else:
                verification_message = "Invalid OTP!"
```

#### HTML CODE
``` HTML
    # Render template with form and feedback message
    return render_template_string('''
    <h1>Liquid Petroleum Gas</h1>
    <h2>LPG Home Delivery</h2>
    
    <!-- OTP Sending Form -->
    <form method="post">
        <h3>Enter Customer Details</h3>
        Name: <input type="text" name="name" value="{{ name }}" required><br>
        Phone: <input type="text" name="phone" value="{{ phone }}" required><br>
        Address: <input type="text" name="address" value="{{ address }}" required><br>
        <input type="submit" name="send_otp" value="Send OTP"><br>
    </form>
    
    <p>{{ otp_sent_message }}</p>

    <!-- OTP Verification Form (Phone input appears only after sending OTP) -->
    {% if otp_sent_message %}
    <form method="post">
        <h3>Verify OTP</h3>
        Enter OTP: <input type="text" name="otp" required><br>
        <input type="hidden" name="phone" value="{{ phone }}">
        <input type="submit" name="verify_otp" value="Verify OTP"><br>
    </form>
    {% endif %}
    
    <p>{{ verification_message }}</p>
    <p>{{ delivery_confirmation_message }}</p>
    ''', otp_sent_message=otp_sent_message, verification_message=verification_message, delivery_confirmation_message=delivery_confirmation_message, name=name, phone=phone, address=address)

if __name__ == '__main__':
    print("Your server is running on http://127.0.0.1:5000")
    serve(app, host='127.0.0.1', port=5000)
```

## CONCLUSION
### This project covered the important software development topics like, OOP PRICIPLES, BACKEND SERVER INTERACTION, DATA HANDLING, VALIDATION.

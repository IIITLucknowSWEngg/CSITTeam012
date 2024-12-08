# 1) User Registration Test

## Feature: User Registration

### Scenario: User registers successfully

#### Description:
Test the user registration process to ensure users can register successfully and are redirected to the login page upon completion.


### Steps:

1. **Given**:  
   The user is on the registration page.

2. **When**:  
   The user enters valid details such as valid name, email, password

3. **Then**:  
   - The user sees a success message: `"Registration successful"`.  
   - The user is redirected to the login page (`/login`).


## Chai.js Code:
```javascript
const chai = require('chai');
const expect = chai.expect;
const registrationPage = require('../pages/registrationPage');

describe('BookMyShow User Registration', function() {
  it('should register the user successfully', function() {
    registrationPage.open();
    registrationPage.fillRegistrationForm(
      'Priyanshu Agarwal',
      'priyanshu@example.com',
      'securePassword123'
    );
    registrationPage.submitForm();
    expect(registrationPage.getSuccessMessage()).to.equal('Registration successful');
    expect(browser.getUrl()).to.include('/login');
  });
});
```
# 2) User Login Test

## Feature: User Login

### Scenario: User logs in with valid credentials

#### Description:
Test the user login process to ensure users can log in successfully and are redirected to the dashboard upon successful authentication.


### Steps:

1. **Given**:  
   The user is on the login page.

2. **When**:  
   The user enters valid credentials such as email and password.

3. **Then**:  
   - The user sees a success message: `"Login successful"`.  
   - The user is redirected to the login page (`/login`).

## Chai.js Code:
```javascript
const chai = require('chai');
const expect = chai.expect;
const loginPage = require('../pages/loginPage');

describe('BookMyShow User Login', function() {
  it('should login user successfully', function() {
    loginPage.open();
    loginPage.enterCredentials('priyanshu@example.com', 'password123');
    loginPage.submitLogin();
    expect(loginPage.getWelcomeMessage()).to.include('Welcome, Priyanshu Agarwal');
    expect(browser.getUrl()).to.include('/dashboard');
  });
});
```

# 3) Event and Movie Management Test

## Feature: Event and Movie Management

### Scenario: User views events and movies, and checks movie schedules

#### Description:
Test the event and movie management functionality to ensure that events and movies are listed, categorized, and scheduled correctly, and that the user can search for events by city or venue.


### Steps:

1. **Given**:  
   The user is on the event or movie listing page.

2. **When**:  
   - The user views the event or movie listings.  
   - The user sorts or filters the events or movies based on category (e.g., genre, language) or venue.  
   - The user searches for events or movies by city or venue.  
   - The user checks the movie schedule for available shows and seat availability.

3. **Then**:  
   - The events or movies should be correctly categorized and sorted.  
   - The user should receive accurate search results based on the city or venue.  
   - Movie schedules should be displayed accurately with available seats for the selected showtime.  
   - Expired schedules should not be available for booking.


## Chai.js Code:
```javascript
const chai = require('chai');
const expect = chai.expect;
const eventPage = require('../pages/eventPage');
const moviePage = require('../pages/moviePage');

describe('BookMyShow Event and Movie Management', function() {
  it('should display events sorted by category and city', function() {
    eventPage.open();
    eventPage.sortEvents('Category');
    eventPage.filterEventsByCity('Mumbai');
    const events = eventPage.getEventList();
    expect(events[0].category).to.equal('Concert');
    expect(events[0].city).to.equal('Mumbai');
  });

  it('should display accurate movie schedules and seat availability', function() {
    moviePage.open();
    moviePage.searchMovie('Pushpa 2');
    moviePage.selectShowtime('7:00 PM');
    const availableSeats = moviePage.getAvailableSeats('7:00 PM');
    expect(availableSeats).to.be.greaterThan(0);
  });

  it('should not display expired movie schedules', function() {
    moviePage.open();
    moviePage.searchMovie('Pushpa 2');
    const expiredSchedules = moviePage.getExpiredSchedules();
    expect(expiredSchedules.length).to.equal(0);
  });
});
```

# 4) Ticket Booking Test

## Feature: Ticket Booking

### Scenario: User books a ticket successfully

#### Description:
Test the ticket booking functionality to ensure users can successfully book tickets for a movie or event and receive a confirmation message.


### Steps:

1. **Given**:  
   - The user is logged in.  
   - The user is on the ticket booking page.

2. **When**:  
   - The user selects a movie or event.  
   - The user selects the showtime and seats.  
   - The user selects a payment method.

3. **Then**:  
   - The ticket should be successfully booked.  
   - The user sees a confirmation message: `"Ticket booked successfully"`.  
   - The user is redirected to the booking details page (`/booking-details`).


## Chai.js Code:
```javascript
const chai = require('chai');
const expect = chai.expect;
const bookingPage = require('../pages/bookingPage');

describe('BookMyShow Ticket Booking', function() {
  it('should book a ticket successfully', function() {
    bookingPage.open();
    bookingPage.selectMovie('Pushpa 2');
    bookingPage.selectShowtime('7:00 PM');
    bookingPage.selectSeats(['A1', 'A2']);
    bookingPage.selectPaymentMethod('Credit Card');
    bookingPage.submitBooking();
    expect(bookingPage.getConfirmationMessage()).to.equal('Ticket booked successfully');
    expect(browser.getUrl()).to.include('/booking-details');
  });
});
```

# 5) Payment Processing Test

## Feature: Payment Processing

### Scenario: User completes a payment for booking tickets

#### Description:
Test the payment process to ensure users can successfully complete their payments for ticket bookings and receive a payment confirmation.

---

### Steps:

1. **Given**:  
   The user has selected tickets and is on the payment page.

2. **When**:  
   The user selects a payment method and enters valid payment details -: Card Number, Expiry Date, CVV

3. **Then**:  
   - The payment should be processed successfully.  
   - The user receives a confirmation message: `"Payment successful"`.  
   - The user is redirected to the payment confirmation page (`/payment-confirmation`).


## Chai.js Code:

```javascript
const chai = require('chai');
const expect = chai.expect;
const paymentPage = require('../pages/paymentPage');

describe('BookMyShow Payment Processing', function() {
  it('should process payment successfully', function() {
    paymentPage.open();
    paymentPage.enterPaymentDetails('1234 5678 9012 3456', '12/25', '123');
    paymentPage.submitPayment();
    expect(paymentPage.getPaymentConfirmation()).to.equal('Payment successful');
    expect(browser.getUrl()).to.include('/payment-confirmation');
  });
});
```
# 6) Cancellations and Refunds Test

## Feature: Cancellations and Refunds

### Scenario: User cancels a booking and receives a refund

#### Description:
Test the cancellation and refund process to ensure users are properly refunded based on the cancellation policy, and that the system provides timely notifications.


### Steps:

1. **Given**:  
   The user has a confirmed ticket booking.

2. **When**:  
   - The user decides to cancel the booking.  
   - The user is informed of the refund amount based on the cancellation time.

3. **Then**:  
   - The refund amount should be correctly calculated according to the cancellation policy.  
   - The user should receive a notification about the cancellation and refund status.  
   - The refund should be processed and reflected in the user’s payment method within the defined timeline.



## Chai.js Code:

```javascript
const chai = require('chai');
const expect = chai.expect;
const bookingPage = require('../pages/bookingPage');
const refundPage = require('../pages/refundPage');

describe('BookMyShow Cancellations and Refunds', function() {
  it('should process refund correctly based on cancellation time', function() {
    bookingPage.open();
    bookingPage.bookTicket('Pushpa 2', '7:00 PM', ['A1', 'A2']);
    bookingPage.cancelBooking();
    
    refundPage.open();
    refundPage.checkRefundAmount('7:00 PM');
    
    const expectedRefundAmount = 500;
    expect(refundPage.getRefundAmount()).to.equal(expectedRefundAmount);
    expect(refundPage.getRefundStatus()).to.include('Refund processed successfully');
    
    refundPage.processRefund('Credit Card');
    expect(refundPage.getRefundStatus()).to.include('Refund completed within 3-5 business days');
  });
});
```

# 7) User Profile Management Test

## Feature: User Profile Management

### Scenario: User updates their profile successfully

#### Description:
Test the profile update functionality to ensure users can modify their profile details (email, password, first name, last name, and address), and the changes are saved successfully.


### Steps:

1. **Given**:  
   The user is logged in.

2. **When**:  
   - The user navigates to the profile page.  
   - The user updates their profile information with the details such as Email, Password, FirstName, LastName and Address.

3. **Then**:  
   - The profile should be updated successfully.  
   - The user sees a confirmation message: `"Profile updated successfully"`.



## Chai.js Code:

```javascript
const chai = require('chai');
const expect = chai.expect;
const profilePage = require('../pages/profilePage');

describe('BookMyShow User Profile Management', function() {
  it('should update user profile successfully', function() {
    profilePage.open();
    profilePage.updateProfile({
      email: 'priyanshu@example.com',
      password: 'newSecurePassword123',
      firstName: 'Priyanshu',
      lastName: 'Updated',
      address: '123 New Street, Uttar Pradesh, India'
    });
    expect(profilePage.getSuccessMessage()).to.equal('Profile updated successfully');
  });
});
```

# 8) Security Compliance Testing Scenario

## Scenario: GDPR Compliance - User Data Deletion

### Description:
This test ensures that users can request the deletion of their personal data as per **GDPR Article 17 (Right to Erasure)**. The platform must process these requests securely and return appropriate responses confirming the deletion.

### Chai.js Code:

```javascript
const chai = require('chai');
const chaiHttp = require('chai-http');
const server = require('../server'); 
const { expect } = chai;

chai.use(chaiHttp);

describe('GDPR Compliance: User Data Deletion', () => {
    it('should delete user personal data upon request', (done) => {
        chai.request(server)
            .delete('/api/users/<userName/userId>') 
            .set('Authorization', `Bearer validAuthToken`) 
            .end((err, res) => {
                expect(res).to.have.status(200); // Expect HTTP 200 for success
                expect(res.body).to.have.property('message', 'User data deleted successfully');
                done();
            });
    });

    it('should return an error for unauthorized deletion requests', (done) => {
        chai.request(server)
            .delete('/api/users/me') // API endpoint for user data deletion
            .end((err, res) => {
                expect(res).to.have.status(401); // Expect HTTP 401 for unauthorized
                expect(res.body).to.have.property('error', 'Unauthorized');
                done();
            });
    });
});
```

# 9) Advertisements and Banners Visiblity Testing 

## Scenario: Ad Display and Performance Tracking

### Description:
This test ensures that advertisements and sponsor banners are displayed correctly on the platform and that their performance (clicks, impressions) is accurately tracked.

### Chai.js Code :

```javascript
const chai = require('chai');
const chaiHttp = require('chai-http');
const server = require('../server');
const { expect } = chai;

chai.use(chaiHttp);

describe('Ad Display and Performance Tracking', () => {
    it('should display the correct ad banner based on the placement', (done) => {
        chai.request(server)
            .get('/api/ads/placement/homepage') {
            .end((err, res) => {
                expect(res).to.have.status(200);
                expect(res.body).to.have.property('adId'); // Ensure adId is returned
                expect(res.body).to.have.property('imageUrl').that.is.a('string'); 
                expect(res.body).to.have.property('placement', 'homepage');
                done();
            });
    });

    it('should accurately track ad clicks', (done) => {
        chai.request(server)
            .post('/api/ads/click')
            .send({ adId: 'ad12345', userId: 'user67890' }) 
            .end((err, res) => {
                expect(res).to.have.status(200);
                expect(res.body).to.have.property('message', 'Click tracked successfully');
                done();
            });
    });

    it('should return an error for invalid ad click requests', (done) => {
        chai.request(server)
            .post('/api/ads/click')
            .send({ adId: '', userId: 'user67890' }) 
            .end((err, res) => {
                expect(res).to.have.status(400);
                expect(res.body).to.have.property('error', 'Invalid adId');
                done();
            });
    });
});
```

# 10) Advertisements Accuracy and Delivery Compliance.

## Scenario: Ad Targeting Accuracy and Delivery Compliance

### Description:
This test ensures that the platform delivers ads to the correct audience based on predefined targeting criteria, such as user demographics, location, or preferences. It also validates that the platform provides accurate delivery reports to advertisers.

### Test Script:

```javascript
// Import necessary libraries
const chai = require('chai');
const chaiHttp = require('chai-http');
const server = require('../server'); 
const { expect } = chai;

chai.use(chaiHttp);

describe('Ad Targeting Accuracy and Delivery Compliance', () => {
    it('should deliver ads to users matching the targeting criteria', (done) => {
        const userMock = {
            userId: '12345',
            demographics: { age: 25, location: 'Lucknow', interests: ['movies', 'music'] }
        };

        chai.request(server)
            .post('/api/ads/serve')
            .send(userMock) 
            .end((err, res) => {
                expect(res).to.have.status(200); 
                expect(res.body).to.have.property('ad').that.includes('adId'); 
                expect(res.body.ad).to.have.property('targetingCriteria');
                expect(res.body.ad.targetingCriteria).to.include({ location: 'Lucknow' }); 
                done();
            });
    });

    it('should log ad impressions and provide accurate reporting', (done) => {
        const adId = 'ad123';

        chai.request(server)
            .get(`/api/ads/report/${adId}`)
            .end((err, res) => {
                expect(res).to.have.status(200); 
                expect(res.body).to.have.property('impressions').that.is.a('number'); 
                expect(res.body.impressions).to.be.above(0); 
                done();
            });
    });

    it('should reject ads with invalid targeting parameters', (done) => {
        const invalidAd = {
            adId: 'invalid123',
            targetingCriteria: { age: 'invalidAge', location: 12345 }
        };

        chai.request(server)
            .post('/api/ads/validate')
            .send(invalidAd)
            .end((err, res) => {
                expect(res).to.have.status(400); 
                expect(res.body).to.have.property('error', 'Invalid targeting parameters');
                done();
            });
    });
});
```







# QA Interview Test - Login Application

This Vue 3 login application is designed for QA testing interviews. It provides a realistic scenario for candidates to demonstrate their testing skills using Playwright or Ranorex.

## Test Credentials

- **Email**: `test@example.com`
- **Password**: `Password123!`

## Features to Test

### Login Form

- Email format validation
- Password complexity requirements (8+ chars, uppercase, lowercase, number, special char)
- Real-time validation feedback
- Loading states during submission
- Error handling for invalid credentials

### Dashboard

- Welcome message display
- Login timestamp
- Logout functionality

### Navigation

- Login success redirects to dashboard
- Logout returns to login form

## Key Test Scenarios

1. **Valid/Invalid Credentials**: Test successful login and various failure cases
2. **Field Validation**: Test empty fields, invalid formats, and requirement violations
3. **UI Behavior**: Verify loading states, error messages, and button states
4. **Form Submission**: Test Enter key submission and button clicks
5. **Navigation Flow**: Complete login/logout cycle

## Detailed Test Cases

### 1. Input Validation Tests

- [ ] Empty email field validation
- [ ] Invalid email format validation
- [ ] Email too long (>254 characters)
- [ ] Empty password field validation
- [ ] Password too short (<8 characters)
- [ ] Password too long (>128 characters)
- [ ] Password missing uppercase letter
- [ ] Password missing lowercase letter
- [ ] Password missing number
- [ ] Password missing special character

### 2. Authentication Tests

- [ ] Valid credentials login success
- [ ] Invalid email login failure
- [ ] Invalid password login failure
- [ ] Case sensitivity testing (email should be case-insensitive)
- [ ] Whitespace handling in email field

### 3. UI/UX Tests

- [ ] Form submission with Enter key
- [ ] Button disabled states during validation errors
- [ ] Button disabled state during loading
- [ ] Loading indicator display
- [ ] Error message display and clearing
- [ ] Real-time validation feedback

### 4. Navigation Tests

- [ ] Successful login redirects to dashboard
- [ ] Dashboard displays correct username
- [ ] Logout returns to login page
- [ ] Login timestamp is displayed correctly

### 5. Browser Compatibility Tests

- [ ] Cross-browser functionality
- [ ] Mobile responsiveness
- [ ] Form autofill behavior

## Test Data Attributes for Automation

All interactive elements have `data-testid` attributes for easy automation:

- `login-form`: Main login container
- `username-input`: Email input field
- `password-input`: Password input field
- `login-button`: Login submit button
- `username-error`: Username validation error
- `password-error`: Password validation error
- `login-error-message`: General login error
- `dashboard`: Dashboard container
- `welcome-message`: Welcome text
- `logout-button`: Logout button

## Expected Behaviors

1. **Form Validation**: Real-time validation should trigger as user types
2. **Error Clearing**: Errors should clear when user starts correcting input
3. **Loading State**: 1.5 second simulated API delay with loading indicator
4. **Session Management**: User remains logged in until logout is clicked
5. **Accessibility**: Proper labels and form structure

## Automation Testing Tips

- Use the provided test data attributes for reliable element selection
- Test both positive and negative scenarios
- Verify error message content and timing
- Test keyboard navigation and form submission
- Validate visual feedback (colors, disabled states)

## Notes for Interviewers

This application includes realistic behaviors like:

- API simulation with loading states
- Complex validation rules
- Edge case handling
- Proper error messaging
- Accessible markup

The test scenarios range from basic functional tests to edge cases, allowing candidates to demonstrate various levels of testing expertise.

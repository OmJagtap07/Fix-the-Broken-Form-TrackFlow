# Bug Report

## Bug #1: Empty Submission
**Description:** The form can be submitted even if all required fields are left blank.
**Root Cause:** The `validate()` function currently contains no validation logic and hardcodes a return value of `true`. Furthermore, `handleSubmit()` invokes `validate()` but ignores its return value, allowing the code execution to proceed to the API call regardless of validation outcome.

## Bug #2: Double Submission
**Description:** A user can click the "Submit Bug Report" button multiple times quickly, causing multiple API requests to fire.
**Root Cause:** The `loading` state is defined but never updated to `true` before the API call `await submitBugReport()`. Additionally, the submit `<button>` does not use the `loading` state for its `disabled` attribute, which means the button remains active and clickable during in-flight network requests.

## Bug #3: Form Not Cleared
**Description:** After a successful submission and the appearance of the success banner, the form fields retain their previous values instead of resetting.
**Root Cause:** In the success path of `handleSubmit()` (after the await resolves), there is no call to `setForm()` to reset the form fields to their initial empty values.

## Bug #4: Silent Server Error
**Description:** If the title contains the word "login", the server rejects the request. However, the user is not notified, and the error fails silently.
**Root Cause:** The `catch (err)` block inside `handleSubmit()` is completely empty. The rejected promise from the mock API is caught, but it is never mapped to `setServerError` or `setErrors` state to inform the user.

## Bug #5: No Field-Level Messages
**Description:** When specific fields fail validation (e.g., submitting with an empty title), no error message appears next to the specific failing field.
**Root Cause:** The JSX does not conditionally render any error messages based on the `errors` state object. Even if `errors` were populated, there is no code in the markup to display them under their respective inputs.

## Bug #6: Invalid Step Count
**Description:** The "No. of Steps" field accepts 0, negative numbers, and empty values.
**Root Cause:** There is no specific check inside the `validate()` function to ensure that `stepsCount` is a valid positive integer greater than 0 before submission.

---
**Live URL:** (To be added after deployment)

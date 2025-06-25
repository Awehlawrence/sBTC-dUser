

# sBTC-dUser - Decentralized Social Login Smart Contract

**Version:** 1.0.0
**Language:** Clarity
**Platform:** Stacks Blockchain

## 📘 Description

This Clarity smart contract implements a **decentralized identity and social login system** that allows users to register, update, and manage their user profiles on-chain.

The contract offers:

* Unique username-based registration
* Email and optional profile image storage
* Validation logic for input fields
* Username availability checking
* Profile update and deletion
* Secure on-chain identity storage without centralized servers

---

## ✨ Features

### 🧑 User Registration

* Users can register with a **unique username** and **email address**
* Validates username length (3–50 characters) and format
* Validates email format (5–100 characters with `@` and `.` required)

### 🖼️ Profile Image

* Users can add or clear an optional profile image URL
* URLs are validated and stored using `(optional (string-utf8 256))`

### 🔁 Profile Updates

* Users can update their **username** and **email**
* Ensures new usernames are not already taken
* Automatically removes the old username from the taken list

### 🗑️ Account Management

* Users can delete their account
* Automatically decrements the user count and clears their data

### 🔍 Username Availability

* Check if a username is available before registering
* Validates format and length before responding

### 📊 Analytics

* Tracks total number of registered users
* Can retrieve full user profile by principal

---

## 🗃️ Data Structures

### `users (principal => user-data)`

```clojure
{
  username: (string-ascii 50),
  email: (string-ascii 100),
  profile-image: (optional (string-utf8 256))
}
```

### `taken-usernames (string-ascii 50 => bool)`

Marks whether a username is already taken.

### `user-count (uint)`

Tracks total number of users registered on-chain.

---

## ⚙️ Public Functions

| Function                                  | Description                      |
| ----------------------------------------- | -------------------------------- |
| `register-user(username, email)`          | Register a new user profile      |
| `update-profile(new-username, new-email)` | Update existing user profile     |
| `delete-profile()`                        | Delete caller's profile          |
| `set-profile-image(image-url)`            | Set or change profile image      |
| `clear-profile-image()`                   | Remove current profile image     |
| `is-username-available(username)`         | Check if a username is available |

---

## 📖 Read-only Functions

| Function                   | Description                          |
| -------------------------- | ------------------------------------ |
| `get-user-info(user)`      | Get full user profile data           |
| `get-user-count()`         | Get total number of registered users |
| `is-user-registered(user)` | Check if a principal has a profile   |

---

## ⚠️ Error Handling

| Error Code                                                                      | Description                                           |
| ------------------------------------------------------------------------------- | ----------------------------------------------------- |
| `"User already exists"`                                                         | Attempting to register a profile that already exists  |
| `"User not found"`                                                              | Trying to update/delete a non-existent user           |
| `"Invalid username: must be between 3 and 50 characters"`                       | Username length is invalid                            |
| `"Invalid email: must be between 5 and 100 characters and contain '@' and '.'"` | Email format or length is invalid                     |
| `"Invalid image URL: must be a valid URL string"`                               | Profile image URL is invalid                          |
| `"Username is already taken"`                                                   | Attempting to use a username that is already reserved |

---

## ✅ Usage Flow

1. **User Registration**

   ```clojure
   (register-user "kenny01" "kenny@mail.com")
   ```

2. **Set Profile Image**

   ```clojure
   (set-profile-image "https://myprofilepic.com/kenny.png")
   ```

3. **Update Profile**

   ```clojure
   (update-profile "kenny02" "newkenny@mail.com")
   ```

4. **Check Username Availability**

   ```clojure
   (is-username-available "kenny02")
   ```

5. **Delete Profile**

   ```clojure
   (delete-profile)
   ```

---

## 🔐 Security Notes

* The contract ensures all profile changes are initiated by the user (`tx-sender`) themselves.
* `unwrap-panic` is used where safe input validation has already occurred.
* Usernames are tracked in a separate map to enforce uniqueness even if a user deletes their account.

---
# Firebase Security Rules Documentation

## Overview

These Firebase Security Rules provide comprehensive validation and access control for the Vibe Coding NYU ride sharing app. The rules are designed for a development environment without authentication, allowing read/write access to all users while maintaining data integrity through strict validation.

## Key Features

- ✅ **Open Access**: Read/write access for all users (no authentication required)
- ✅ **Data Validation**: Strict validation for all data types
- ✅ **Auto Timestamps**: Enforced timestamp validation
- ✅ **Negative Value Prevention**: Prevents negative seats, ratings, and counts
- ✅ **Immutable Fields**: Protects critical fields from modification
- ✅ **Business Logic**: Enforces app-specific business rules

## Collections and Rules

### 1. Users Collection (`/users/{userId}`)

**Access**: Read/write for all users

**Validation Rules**:
- `userId` must be a non-empty string
- `name` must be a non-empty string
- `email` must be a valid email format
- `averageRating` must be >= 0
- `ridesGiven` must be >= 0
- `ridesTaken` must be >= 0
- `ratingCount` must be >= 0
- `createdAt` and `updatedAt` must be valid timestamps
- `userId` and `createdAt` cannot be modified after creation

**Required Fields**:
```json
{
  "name": "string",
  "email": "string",
  "userId": "string",
  "averageRating": 0,
  "ridesGiven": 0,
  "ridesTaken": 0,
  "ratingCount": 0,
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}
```

### 2. Trips Collection (`/trips/{tripId}`)

**Access**: Read/write for all users

**Validation Rules**:
- `driverId` must be a valid user ID
- `from` and `to` must be non-empty strings
- `date` must be a valid date string
- `time` must be in HH:MM format
- `totalSeats` must be > 0
- `seatsLeft` must be >= 0 and <= `totalSeats`
- `status` must be one of: 'active', 'completed', 'cancelled'
- Coordinates (if provided) must have valid latitude, longitude, and address
- `createdAt` and `updatedAt` must be valid timestamps

**Required Fields**:
```json
{
  "driverId": "string",
  "from": "string",
  "to": "string",
  "date": "string",
  "time": "string",
  "totalSeats": "number",
  "seatsLeft": "number",
  "status": "string",
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}
```

**Optional Fields**:
```json
{
  "fromCoordinates": {
    "latitude": "number",
    "longitude": "number",
    "address": "string"
  },
  "toCoordinates": {
    "latitude": "number",
    "longitude": "number",
    "address": "string"
  },
  "notes": "string"
}
```

### 3. Bookings Collection (`/bookings/{bookingId}`)

**Access**: Read/write for all users

**Validation Rules**:
- `tripId` and `passengerId` must be valid user IDs
- `status` must be one of: 'pending', 'confirmed', 'declined'
- `createdAt` and `updatedAt` must be valid timestamps
- `tripId`, `passengerId`, and `createdAt` cannot be modified after creation

**Required Fields**:
```json
{
  "tripId": "string",
  "passengerId": "string",
  "status": "string",
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}
```

### 4. Ratings Collection (`/ratings/{ratingId}`)

**Access**: Read/write for all users

**Validation Rules**:
- `tripId`, `raterId`, and `rateeId` must be valid user IDs
- `rating` must be between 1 and 5 (inclusive)
- `role` must be either 'driver' or 'passenger'
- `raterId` cannot be the same as `rateeId` (no self-rating)
- `createdAt` must be a valid timestamp
- **Immutable**: Once created, ratings cannot be updated or deleted

**Required Fields**:
```json
{
  "tripId": "string",
  "raterId": "string",
  "rateeId": "string",
  "rating": "number",
  "role": "string",
  "createdAt": "timestamp"
}
```

**Optional Fields**:
```json
{
  "comment": "string"
}
```

### 5. Posts Collection (`/posts/{postId}`)

**Access**: Read/write for all users

**Validation Rules**:
- `title` and `content` must be non-empty strings
- `authorId` must be a valid user ID
- `createdAt` and `updatedAt` must be valid timestamps
- `authorId` and `createdAt` cannot be modified after creation

**Required Fields**:
```json
{
  "title": "string",
  "content": "string",
  "authorId": "string",
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}
```

## Helper Functions

### `isValidTimestamp(timestamp)`
- Validates that the timestamp is a valid Firestore timestamp
- Ensures the timestamp is not in the future

### `isValidRating(rating)`
- Validates that the rating is a number between 1 and 5 (inclusive)
- Prevents invalid ratings like 0, negative numbers, or values > 5

### `isValidSeatCount(seats)`
- Validates that seat counts are non-negative numbers
- Prevents negative seat counts

### `isValidUserId(userId)`
- Validates that user IDs are non-empty strings
- Ensures user IDs are properly formatted

### `isValidEmail(email)`
- Validates email format using regex pattern
- Ensures emails contain @ and domain

### `isValidDate(dateString)`
- Validates that date strings are non-empty
- Ensures dates are properly formatted

### `isValidTime(timeString)`
- Validates time format as HH:MM
- Ensures times are in 24-hour format

## Business Logic Rules

### 1. Seat Management
- `totalSeats` must always be > 0
- `seatsLeft` must always be >= 0
- `seatsLeft` cannot exceed `totalSeats`
- Updates maintain seat count integrity

### 2. Rating System
- Ratings must be between 1-5 stars
- Users cannot rate themselves
- Ratings are immutable once created
- Rating data includes trip context

### 3. Booking System
- Bookings reference valid trips and users
- Status transitions are controlled
- Critical fields are immutable after creation

### 4. Data Integrity
- Timestamps are automatically validated
- Required fields are enforced
- Data types are strictly validated
- Business rules are enforced at the database level

## Security Considerations

### Current State (Development)
- **Open Access**: All users can read/write all data
- **No Authentication**: Rules don't check user identity
- **Data Validation**: Comprehensive validation prevents bad data
- **Business Logic**: App-specific rules are enforced

### Future Authentication Integration
When authentication is added, update rules to:
```javascript
// Example for authenticated users
allow read, write: if request.auth != null;

// Example for user-specific data
allow read, write: if request.auth != null && 
  request.auth.uid == resource.data.userId;
```

## Deployment

### Deploy Rules
```bash
# Deploy Firestore rules
firebase deploy --only firestore:rules

# Deploy rules and indexes
firebase deploy --only firestore
```

### Test Rules
```bash
# Run rules tests
firebase emulators:start --only firestore
```

## Error Handling

### Common Validation Errors
1. **Invalid Ratings**: Ratings must be 1-5
2. **Negative Seats**: Seat counts cannot be negative
3. **Invalid Timestamps**: Timestamps must be valid and not in future
4. **Missing Fields**: Required fields must be present
5. **Immutable Fields**: Critical fields cannot be modified

### Debugging
- Check Firestore console for rule violations
- Use Firebase emulator for local testing
- Review rule evaluation logs for detailed error information

## Migration to Production

When moving to production:

1. **Enable Authentication**
2. **Update Access Rules**: Replace `if true` with proper auth checks
3. **Add User-Specific Rules**: Implement proper data ownership
4. **Review Validation**: Ensure all business rules are appropriate
5. **Test Thoroughly**: Validate all rule changes with comprehensive tests

## Best Practices

1. **Always Validate**: Never trust client-side validation alone
2. **Immutable Critical Data**: Protect important fields from modification
3. **Business Logic in Rules**: Enforce app rules at the database level
4. **Comprehensive Testing**: Test all rule scenarios thoroughly
5. **Documentation**: Keep rules well-documented and maintained





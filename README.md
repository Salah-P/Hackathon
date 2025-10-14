# Vibe Coding NYU - Flutter App

A Flutter application with Firebase Firestore integration featuring a modern navigation structure and multiple screens.

## Features

- **User Registration**: First-time user onboarding with name and email collection
- **Unique User ID**: Automatically generated UUID for each user
- **Local Storage**: User data persistence using SharedPreferences
- **Firebase Firestore Integration**: Complete CRUD operations for posts and user data
- **User Profile Management**: Display and edit user information with ride statistics
- **Modern UI**: Material Design 3 with responsive navigation
- **Multi-Screen Architecture**: Welcome, Home, Trips, Rides, Driver Bookings, Completed Trips, Profile, and Settings screens
- **Google Maps Integration**: Interactive maps with location picker and route display
- **State Management**: Provider pattern for efficient state management
- **Real-time Updates**: Live data synchronization with Firestore
- **Push Notifications**: Firebase Cloud Messaging for booking updates and trip reminders
- **Cloud Functions**: Automated notification system for drivers and passengers

## Project Structure

```
lib/
├── main.dart                    # App entry point and navigation
├── services/
│   ├── firebase_service.dart    # Firebase Firestore operations
│   ├── local_storage_service.dart # Local data persistence
│   └── fcm_service.dart        # Firebase Cloud Messaging service
└── screens/
    ├── welcome_screen.dart      # First-time user onboarding
    ├── home_screen.dart         # Main screen with posts
    ├── trips_screen.dart        # Browse available trips with search
    ├── rides_screen.dart        # Browse and manage rides
    ├── post_ride_screen.dart    # Driver ride posting form
    ├── driver_bookings_screen.dart # Driver booking request management
    ├── completed_trips_screen.dart # Completed trips and rating management
    ├── rating_screen.dart       # Rating interface with stars and comments
    ├── trip_details_screen.dart # Trip details with map and route display
    ├── profile_screen.dart      # User profile management
    └── settings_screen.dart     # App settings and configuration

lib/widgets/
├── location_picker.dart         # Google Maps location picker widget
└── loading_widget.dart          # Loading, error, and empty state widgets

functions/
├── src/
│   └── index.ts                # Cloud Functions for notifications
├── package.json               # Node.js dependencies
├── tsconfig.json             # TypeScript configuration
└── README.md                 # Functions documentation
```

## Setup Instructions

### 1. Firebase Configuration

Before running the app, you need to set up Firebase:

1. Go to the [Firebase Console](https://console.firebase.google.com/)
2. Create a new project named "vibe-coding-nyu"
3. Enable Firestore Database
4. Download the configuration files:
   - For Android: Download `google-services.json` and place it in `android/app/`
   - For iOS: Download `GoogleService-Info.plist` and place it in `ios/Runner/`

### 2. Dependencies

The app uses the following main dependencies:

- `firebase_core`: Firebase initialization
- `cloud_firestore`: Firestore database operations
- `firebase_messaging`: Push notifications
- `flutter_local_notifications`: Local notification handling
- `google_maps_flutter`: Interactive maps and location services
- `google_places_flutter`: Google Places autocomplete and search
- `http`: HTTP requests for Google Directions API
- `firebase_storage`: Profile picture uploads and file storage
- `image_picker`: Camera and gallery image selection
- `cached_network_image`: Optimized image loading and caching
- `provider`: State management
- `shared_preferences`: Local data storage
- `uuid`: Unique user ID generation
- `firebase_auth`: User authentication (ready for future implementation)

### 3. Google Maps Setup

1. **Get Google Maps API Key**:
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project or select existing one
   - Enable the following APIs:
     - Maps SDK for Android
     - Maps SDK for iOS
     - Places API
     - Directions API
   - Create credentials (API Key)
   - Restrict the API key to your app's bundle ID

2. **Configure API Key**:
   - Replace `YOUR_GOOGLE_MAPS_API_KEY` in:
     - `android/app/src/main/AndroidManifest.xml`
     - `ios/Runner/AppDelegate.swift`
     - `lib/widgets/location_picker.dart`
     - `lib/screens/trip_details_screen.dart`

### 4. Deploy Cloud Functions

```bash
# Navigate to functions directory
cd functions

# Install dependencies
npm install

# Deploy functions
firebase deploy --only functions
```

### 5. Deploy Firestore Rules

```bash
# Deploy security rules
firebase deploy --only firestore:rules

# Or deploy everything
firebase deploy
```

### 6. Running the App

```bash
# Install dependencies
flutter pub get

# Run the app
flutter run
```

## Navigation Structure

The app now features a modern bottom navigation with four main sections:

### Bottom Navigation Tabs
1. **Browse Rides**: Search and view available trips
2. **My Trips**: Manage trips you've posted as a driver
3. **My Bookings**: View your booking requests as a passenger
4. **Profile**: Manage your profile and settings

## Screen Descriptions

### Welcome Screen (First Launch)
- Collect user name and email on first app launch
- Generate unique user ID using UUID
- Save user data to both local storage and Firestore
- Initialize user profile with default statistics

### Home Screen
- Add new posts with title and content
- View all posts in real-time
- Delete posts with confirmation
- Error handling and loading states

### Trips Screen
- Browse all available trips sorted by date and time
- Search trips by destination
- Filter to show only future trips with available seats
- Display trip details: from, to, date, time, seats available, driver name
- Request seats with detailed trip information modal
- Real-time updates when trips are posted or seats are booked

### Rides Screen
- Browse available rides from other drivers
- View ride details (from, to, date, time, seats available)
- Book seats on available rides
- Manage your own posted rides
- Delete or edit your rides

### Post Ride Screen
- Driver form to post new rides
- Google Maps location picker for From/To locations
- Date and time picker integration
- Available seats and notes input
- Form validation and error handling
- Save trip data to Firestore with coordinates and driver ID

### Driver Bookings Screen
- View booking requests for your trips
- Tabbed interface: Pending, Confirmed, Declined
- Accept or decline booking requests
- Atomic seat reduction when accepting bookings
- Passenger information and trip details
- Real-time updates for booking status changes

### My Trips Screen
- View and manage trips you've posted as a driver
- Tabbed interface: Active, Completed, Cancelled
- Post new rides with floating action button
- View trip details and manage bookings
- Real-time updates for trip status changes

### My Bookings Screen
- View your booking requests as a passenger
- Tabbed interface: Pending, Confirmed, Declined
- Track booking status and trip details
- Access trip information and driver details
- Real-time updates for booking status changes

### Completed Trips Screen
- View trips completed as driver or passenger
- Rate drivers and passengers after trip completion
- Access to trip history and rating interface
- Tabbed interface for driver vs passenger trips
- Integration with rating system

### Trip Details Screen
- Full-screen trip information display
- Interactive Google Maps with route visualization
- From/To markers with custom icons
- Route polyline showing the journey path
- Request seat functionality integrated
- Trip information card with all details

### Rating Screen
- 5-star rating system with visual feedback
- Optional comment section for detailed feedback
- Rating guidelines and tips for users
- Trip information display during rating
- Automatic user statistics updates

### Profile Screen
- Display user information (name, email, user ID)
- Show ride statistics (average rating, rides given, rides taken)
- Edit profile information
- Profile picture upload with camera and gallery options
- View account creation and last update dates
- Copy user ID functionality

### Settings Screen
- App configuration options
- Firebase connection status
- Data management options
- About information

## Services

### Firebase Service
The `FirebaseService` class provides:

- User profile creation and management
- Trip/ride creation and management with coordinates
- Booking request system with status management
- Atomic seat reduction for confirmed bookings
- Rating system with user statistics updates
- Completed trips detection and management
- FCM token management for push notifications
- Generic CRUD operations for any Firestore collection
- Real-time data streams
- Error handling and loading states
- Specific methods for users, posts, trips, bookings, and ratings

### FCM Service
The `FCMService` class provides:

- Firebase Cloud Messaging token management
- Local notification handling
- Background and foreground message processing
- Notification tap handling and navigation
- Topic subscription management
- Scheduled notification support

### Local Storage Service
The `LocalStorageService` class provides:

- User ID persistence
- First launch detection
- User data caching
- Secure local data management

### Location Picker Widget
The `LocationPicker` widget provides:

- Google Maps integration with search functionality
- Google Places autocomplete for location search
- Interactive map for manual location selection
- Coordinate storage and address display
- Modal interface with search and map selection

### Storage Service
The `StorageService` class provides:

- Firebase Storage integration for file uploads
- Profile picture upload and management
- Image compression and optimization
- File deletion and cleanup
- Error handling for storage operations

### Loading Widgets
The `LoadingWidget` components provide:

- Consistent loading indicators across the app
- Loading dialogs with customizable messages
- Error state displays with retry functionality
- Empty state widgets for better UX
- Standardized loading and error handling

## Push Notifications

The app includes a comprehensive push notification system:

### Notification Types

1. **Booking Request Notifications**
   - Sent to drivers when passengers request seats
   - Includes passenger name and trip details

2. **Booking Response Notifications**
   - Sent to passengers when drivers accept/decline requests
   - Confirmation or decline messages with trip information

3. **Trip Reminder Notifications**
   - Sent 2 hours before trip departure
   - Sent to both drivers and confirmed passengers
   - Includes trip details and timing

### Cloud Functions

- **onBookingCreated**: Triggers when new booking requests are created
- **onBookingUpdated**: Triggers when booking status changes
- **sendTripReminders**: Scheduled function that runs every 30 minutes
- **detectCompletedTrips**: Detects completed trips and sends rating prompts
- **onRatingCreated**: Updates user statistics when ratings are submitted
- **cleanupExpiredTokens**: Daily cleanup of invalid FCM tokens

## Google Maps Features

The app includes comprehensive Google Maps integration:

### Location Picker
- **Search Functionality**: Google Places autocomplete for finding locations
- **Interactive Map**: Tap on map to select precise locations
- **Address Display**: Shows selected address with coordinates
- **Modal Interface**: Full-screen picker with search and map

### Trip Details Screen
- **Route Visualization**: Shows the complete route from origin to destination
- **Custom Markers**: Green marker for origin, red marker for destination
- **Interactive Map**: Zoom, pan, and explore the route
- **Auto-fit Camera**: Automatically fits the entire route in view
- **Request Integration**: Request seat button directly on map screen

### Data Storage
- **Coordinates**: Stores latitude/longitude for all locations
- **Addresses**: Maintains human-readable addresses
- **Route Data**: Calculates and displays optimal routes
- **Location History**: Previous selections are remembered

## Future Enhancements

- User authentication with Firebase Auth
- Real-time location tracking during trips
- Advanced route optimization and alternatives
- Image upload functionality
- Advanced offline support with map caching
- Real-time chat between drivers and passengers
- Payment integration
- Advanced analytics and reporting
- Social features (likes, comments, follows)
- Live trip tracking for passengers

## Security Rules

The app includes comprehensive Firebase Security Rules that provide:

- **Open Access**: Read/write access for all users (development mode)
- **Data Validation**: Strict validation for all data types and formats
- **Auto Timestamps**: Enforced timestamp validation and auto-generation
- **Negative Value Prevention**: Prevents negative seats, ratings, and counts
- **Business Logic**: Enforces app-specific rules at the database level
- **Immutable Fields**: Protects critical fields from unauthorized modification

### Key Validations:
- **Ratings**: Must be between 1-5 stars
- **Seats**: Cannot be negative, seatsLeft ≤ totalSeats
- **Timestamps**: Must be valid and not in the future
- **User IDs**: Must be non-empty strings
- **Email**: Must be valid email format
- **Trip Status**: Must be 'active', 'completed', or 'cancelled'
- **Booking Status**: Must be 'pending', 'confirmed', or 'declined'

See `SECURITY_RULES.md` for detailed documentation.

## Development Notes

- The app uses Material Design 3 theming
- Provider pattern for state management
- Responsive design for different screen sizes
- Error handling with user-friendly messages
- Loading states for better UX

## License

This project is created for educational purposes as part of NYU coursework.

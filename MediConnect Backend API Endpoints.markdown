# MediConnect Backend API Endpoints

This document outlines all backend API endpoints for the MediConnect healthcare platform, organized by role and feature. Each endpoint includes the HTTP method, path, description, and associated controller or service. The endpoints are designed to support the Admin, Patient, and Doctor workflows, along with shared features and integrations, while ensuring security (authentication, authorization, rate limiting) and HIPAA compliance.

## Base URL
All endpoints are relative to the base URL: `/api/v1`

## Authentication Endpoints
**Route Path**: `/auth`  
**Controller**: `authController.js`, `registrationController.js`, `verificationController.js`, `passwordResetController.js`  
**Middleware**: `rateLimiter.js`, `cors.js`, `errorHandler.js`

| Method | Endpoint | Description | Controller |
|--------|----------|-------------|------------|
| POST   | `/auth/register` | Register a new user (Patient/Doctor/Admin) with role selection | `registrationController.register` |
| POST   | `/auth/login` | Authenticate user and return JWT token | `authController.login` |
| POST   | `/auth/refresh-token` | Refresh JWT token | `authController.refreshToken` |
| POST   | `/auth/logout` | Invalidate user session | `authController.logout` |
| GET    | `/auth/verify-email/:token` | Verify user email with token | `verificationController.verifyEmail` |
| POST   | `/auth/resend-verification` | Resend email verification link | `verificationController.resendVerification` |
| POST   | `/auth/forgot-password` | Request password reset link | `passwordResetController.requestReset` |
| POST   | `/auth/reset-password/:token` | Reset password using token | `passwordResetController.resetPassword` |

## Patient Endpoints
**Route Path**: `/patient`  
**Controller**: `patientDashboardController.js`, `appointmentBookingController.js`, `medicalRecordsController.js`, `symptomCheckerController.js`, `patientCommunicationController.js`  
**Middleware**: `authenticate.js`, `roleGuard.js` (Patient role), `patientValidation.js`, `rateLimiter.js`, `cors.js`, `errorHandler.js`

| Method | Endpoint | Description | Controller |
|--------|----------|-------------|------------|
| GET    | `/patient/dashboard` | Fetch patient dashboard data (upcoming appointments, health summary) | `patientDashboardController.getDashboard` |
| GET    | `/patient/doctors` | Search doctors by specialty, location, or availability | `appointmentBookingController.searchDoctors` |
| GET    | `/patient/doctors/:id` | Get doctor profile details | `appointmentBookingController.getDoctorProfile` |
| POST   | `/patient/appointments` | Book a new appointment | `appointmentBookingController.bookAppointment` |
| GET    | `/patient/appointments` | List patient’s appointments | `appointmentBookingController.getAppointments` |
| GET    | `/patient/appointments/:id` | Get details of a specific appointment | `appointmentBookingController.getAppointment` |
| PUT    | `/patient/appointments/:id` | Update appointment (reschedule/cancel) | `appointmentBookingController.updateAppointment` |
| GET    | `/patient/records` | List patient’s medical records | `medicalRecordsController.getRecords` |
| POST   | `/patient/records` | Upload medical document or lab report | `medicalRecordsController.uploadRecord` |
| GET    | `/patient/records/:id` | Get specific medical record | `medicalRecordsController.getRecord` |
| PUT    | `/patient/records/:id` | Update medical record (e.g., add notes) | `medicalRecordsController.updateRecord` |
| DELETE | `/patient/records/:id` | Delete medical record | `medicalRecordsController.deleteRecord` |
| POST   | `/patient/symptom-checker` | Submit symptoms for analysis | `symptomCheckerController.analyzeSymptoms` |
| GET    | `/patient/communication/messages` | Get patient’s chat messages | `patientCommunicationController.getMessages` |
| POST   | `/patient/communication/messages` | Send message to doctor | `patientCommunicationController.sendMessage` |
| GET    | `/patient/communication/emergency` | Get emergency contact details | `patientCommunicationController.getEmergencyContact` |
| POST   | `/patient/profile` | Update patient profile (e.g., medical history, insurance) | `patientDashboardController.updateProfile` |
| GET    | `/patient/profile` | Get patient profile details | `patientDashboardController.getProfile` |

## Doctor Endpoints
**Route Path**: `/doctor`  
**Controller**: `doctorDashboardController.js`, `scheduleManagementController.js`, `consultationController.js`, `prescriptionController.js`, `patientManagementController.js`, `doctorAnalyticsController.js`  
**Middleware**: `authenticate.js`, `roleGuard.js` (Doctor role), `doctorValidation.js`, `rateLimiter.js`, `cors.js`, `errorHandler.js`

| Method | Endpoint | Description | Controller |
|--------|----------|-------------|------------|
| GET    | `/doctor/dashboard` | Fetch doctor dashboard data (today’s schedule, stats) | `doctorDashboardController.getDashboard` |
| GET    | `/doctor/schedule` | Get doctor’s schedule and available time slots | `scheduleManagementController.getSchedule` |
| POST   | `/doctor/schedule` | Create new time slots or update availability | `scheduleManagementController.createSchedule` |
| PUT    | `/doctor/schedule/:id` | Update specific time slot or vacation period | `scheduleManagementController.updateSchedule` |
| DELETE | `/doctor/schedule/:id` | Delete time slot | `scheduleManagementController.deleteSchedule` |
| GET    | `/doctor/patients` | List doctor’s patients | `patientManagementController.getPatients` |
| GET    | `/doctor/patients/:id` | Get patient profile and medical history | `patientManagementController.getPatient` |
| POST   | `/doctor/consultations` | Start a new consultation | `consultationController.startConsultation` |
| GET    | `/doctor/consultations` | List doctor’s consultations | `consultationController.getConsultations` |
| GET    | `/doctor/consultations/:id` | Get consultation details | `consultationController.getConsultation` |
| PUT    | `/doctor/consultations/:id` | Update consultation (e.g., add notes) | `consultationController.updateConsultation` |
| POST   | `/doctor/prescriptions` | Create new prescription | `prescriptionController.createPrescription` |
| GET    | `/doctor/prescriptions` | List prescriptions issued by doctor | `prescriptionController.getPrescriptions` |
| GET    | `/doctor/prescriptions/:id` | Get specific prescription details | `prescriptionController.getPrescription` |
| PUT    | `/doctor/prescriptions/:id` | Update prescription | `prescriptionController.updatePrescription` |
| GET    | `/doctor/analytics` | Fetch practice analytics (patient load, revenue) | `doctorAnalyticsController.getAnalytics` |
| POST   | `/doctor/profile` | Update doctor profile (e.g., specialties, fees) | `doctorDashboardController.updateProfile` |
| GET    | `/doctor/profile` | Get doctor profile details | `doctorDashboardController.getProfile` |
| POST   | `/doctor/patients/:id/messages` | Send message to patient | `patientManagementController.sendMessage` |
| GET    | `/doctor/patients/:id/messages` | Get messages with patient | `patientManagementController.getMessages` |

## Admin Endpoints
**Route Path**: `/admin`  
**Controller**: `adminDashboardController.js`, `userManagementController.js`, `platformAnalyticsController.js`, `contentModerationController.js`, `systemConfigController.js`  
**Middleware**: `authenticate.js`, `roleGuard.js` (Admin role), `adminValidation.js`, `rateLimiter.js`, `cors.js`, `errorHandler.js`

| Method | Endpoint | Description | Controller |
|--------|----------|-------------|------------|
| GET    | `/admin/dashboard` | Fetch admin dashboard data (platform overview, system health) | `adminDashboardController.getDashboard` |
| GET    | `/admin/users` | List all users (Patients, Doctors, Admins) | `userManagementController.getUsers` |
| GET    | `/admin/users/:id` | Get specific user details | `userManagementController.getUser` |
| PUT    | `/admin/users/:id` | Update user account (e.g., suspend/activate) | `userManagementController.updateUser` |
| POST   | `/admin/users/doctors/approve` | Approve doctor application | `userManagementController.approveDoctor` |
| POST   | `/admin/users/doctors/reject` | Reject doctor application | `userManagementController.rejectDoctor` |
| GET    | `/admin/analytics` | Fetch platform analytics (usage, revenue) | `platformAnalyticsController.getAnalytics` |
| GET    | `/admin/analytics/reports` | Generate custom platform reports | `platformAnalyticsController.generateReport` |
| GET    | `/admin/moderation/flagged` | List flagged content or messages | `contentModerationController.getFlaggedContent` |
| PUT    | `/admin/moderation/flagged/:id` | Take action on flagged content (e.g., remove, approve) | `contentModerationController.handleFlaggedContent` |
| GET    | `/admin/moderation/reports` | List user-submitted reports | `contentModerationController.getReports` |
| POST   | `/admin/system/config` | Update system configuration (e.g., email templates, notifications) | `systemConfigController.updateConfig` |
| GET    | `/admin/system/config` | Get system configuration | `systemConfigController.getConfig` |
| POST   | `/admin/system/backup` | Trigger system backup | `systemConfigController.createBackup` |
| GET    | `/admin/system/logs` | Fetch audit logs | `systemConfigController.getLogs` |

## Shared Endpoints
**Route Path**: `/shared`  
**Controller**: `telemedicineController.js`, `chatController.js`, `notificationController.js`, `fileUploadController.js`, `paymentController.js`  
**Middleware**: `authenticate.js`, `roleGuard.js` (varies by role), `rateLimiter.js`, `cors.js`, `errorHandler.js`

| Method | Endpoint | Description | Controller |
|--------|----------|-------------|------------|
| POST   | `/shared/telemedicine/join` | Join telemedicine session (Patient/Doctor) | `telemedicineController.joinSession` |
| GET    | `/shared/telemedicine/:id` | Get telemedicine session details | `telemedicineController.getSession` |
| POST   | `/shared/chat/messages` | Send chat message | `chatController.sendMessage` |
| GET    | `/shared/chat/messages` | Get chat messages for a session | `chatController.getMessages` |
| POST   | `/shared/notifications` | Send notification to user | `notificationController.sendNotification` |
| GET    | `/shared/notifications` | Get user’s notifications | `notificationController.getNotifications` |
| POST   | `/shared/files/upload` | Upload file (e.g., medical document, prescription) | `fileUploadController.uploadFile` |
| GET    | `/shared/files/:id` | Download file by ID | `fileUploadController.getFile` |
| DELETE | `/shared/files/:id` | Delete uploaded file | `fileUploadController.deleteFile` |
| POST   | `/shared/payments` | Process payment for consultation | `paymentController.processPayment` |
| GET    | `/shared/payments/:id` | Get payment details | `paymentController.getPayment` |
| GET    | `/shared/payments` | List user’s payment history | `paymentController.getPayments` |

## Integration Endpoints
**Route Path**: `/integration`  
**Controller**: `insuranceController.js`, `calendarController.js`, `emailController.js`  
**Middleware**: `authenticate.js`, `roleGuard.js` (varies by role), `rateLimiter.js`, `cors.js`, `errorHandler.js`

| Method | Endpoint | Description | Controller |
|--------|----------|-------------|------------|
| POST   | `/integration/insurance/verify` | Verify patient insurance details | `insuranceController.verifyInsurance` |
| GET    | `/integration/insurance/:id` | Get insurance verification status | `insuranceController.getInsuranceStatus` |
| POST   | `/integration/calendar/sync` | Sync appointment with external calendar | `calendarController.syncCalendar` |
| GET    | `/integration/calendar/events` | Get synced calendar events | `calendarController.getCalendarEvents` |
| POST   | `/integration/email/send` | Send custom email (e.g., reminders, announcements) | `emailController.sendEmail` |
| GET    | `/integration/email/templates` | Get email templates | `emailController.getTemplates` |

## WebSocket Endpoints
**Socket Path**: `/sockets`  
**Handlers**: `chatSocketHandler.js`, `appointmentSocketHandler.js`, `notificationSocketHandler.js`, `telemedicineSocketHandler.js`  
**Middleware**: `socketAuth.js`, `socketRateLimit.js`

| Event | Description | Handler |
|-------|-------------|---------|
| `chat.message` | Send/receive real-time chat messages | `chatSocketHandler.handleMessage` |
| `appointment.update` | Real-time appointment updates (e.g., booking, cancellation) | `appointmentSocketHandler.handleUpdate` |
| `notification.send` | Send real-time notifications (e.g., reminders, alerts) | `notificationSocketHandler.sendNotification` |
| `telemedicine.join` | Join telemedicine video call | `telemedicineSocketHandler.joinCall` |
| `telemedicine.leave` | Leave telemedicine video call | `telemedicineSocketHandler.leaveCall` |
| `patient.event` | Patient-specific events (e.g., emergency requests) | `patientEvents.handleEvent` |
| `doctor.event` | Doctor-specific events (e.g., consultation start) | `doctorEvents.handleEvent` |
| `admin.event` | Admin-specific events (e.g., system alerts) | `adminEvents.handleEvent` |

## Security and Compliance
- **Middleware**: All endpoints use `authenticate.js` for JWT-based authentication, `roleGuard.js` for role-based access control, and `rateLimiter.js` for request throttling.
- **Data Protection**: `dataEncryption.js` ensures encrypted data transmission, and `hipaaCompliance.js` enforces HIPAA-compliant data handling.
- **Logging**: `auditLogger.js` logs all actions for compliance, and `requestLogger.js` logs API requests for debugging.
- **Validation**: Role-specific validation (`patientValidation.js`, `doctorValidation.js`, `adminValidation.js`, `appointmentValidation.js`) ensures data integrity.

## Notes
- Endpoints are versioned under `/api/v1` to support future updates.
- All responses follow a standard format: `{ success: boolean, data: object, message: string }`.
- Error handling is managed by `errorHandler.js`, returning appropriate HTTP status codes (e.g., 400, 401, 403, 500).
- File uploads (e.g., medical documents) are handled via `multer.js` and stored in the `uploads/` directory.
- WebSocket events enable real-time features like chat, notifications, and telemedicine.
- Integration endpoints connect with external systems (e.g., insurance, calendars) using `externalApiService.js`.

This endpoint list ensures full coverage of the MediConnect platform’s backend functionality, supporting all workflows and interactions while maintaining security and compliance.
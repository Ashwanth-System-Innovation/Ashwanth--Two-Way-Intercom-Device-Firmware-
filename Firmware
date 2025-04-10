/**
 * Two-way Intercom Device Firmware
 * Model: IC-2000X
 * Version: 3.2.1
 * 
 * Created by: Ashwanth 
 * Last Modified: 2023-11-15
 * 
 * This firmware controls the operation of the IC-2000X two-way intercom system
 * supporting both LAN and PSTN connectivity with central monitoring capabilities.
 */

#include "intercom_system.h"
#include "network_config.h"
#include "audio_processing.h"
#include "security_module.h"
#include "power_management.h"
#include "display_controller.h"
#include "user_interface.h"
#include "call_handling.h"
#include "diagnostics.h"

// System Configuration
#define FIRMWARE_VERSION "3.2.1"
#define DEVICE_MODEL "IC-2000X"
#define MANUFACTURER "SecureTalk Systems"

// Network Configuration
#define DEFAULT_IP_ADDRESS "192.168.1.100"
#define DEFAULT_SUBNET_MASK "255.255.255.0"
#define DEFAULT_GATEWAY "192.168.1.1"
#define PRIMARY_DNS "8.8.8.8"
#define SECONDARY_DNS "8.8.4.4"

// Server Configuration
#define PRIMARY_SERVER "intercom.securetalksystems.com"
#define BACKUP_SERVER "backup.securetalksystems.com"
#define SERVER_PORT 5060
#define HEARTBEAT_INTERVAL 60 // seconds

// PSTN Configuration
#define EMERGENCY_NUMBER "911"
#define SERVICE_CENTER "1-800-555-4321"
#define TECHNICAL_SUPPORT "1-800-555-8765"
#define LOCAL_EXCHANGE_CODE "555"

// Audio Configuration
#define SAMPLE_RATE 16000
#define BIT_DEPTH 16
#define CHANNELS 1
#define NOISE_REDUCTION_LEVEL 3
#define ECHO_CANCELLATION_LEVEL 2
#define GAIN_CONTROL_LEVEL 4

// Global Variables
static SystemState_t systemState;
static NetworkConfig_t networkConfig;
static AudioSettings_t audioSettings;
static SecuritySettings_t securitySettings;
static volatile bool emergencyModeActive = false;
static uint32_t uptime = 0;
static char deviceSerialNumber[16] = "IC2000X12345678";

// Function Prototypes
static void InitializeHardware(void);
static void ConfigureNetwork(void);
static void SetupAudioSubsystem(void);
static void InitializeSecurityModule(void);
static void StartupSelfTest(void);
static void RegisterWithServer(void);
static void HandleIncomingCall(CallInfo_t *callInfo);
static void ProcessUserInput(UserInput_t input);
static void PerformDiagnostics(DiagnosticLevel_t level);
static void EnterEmergencyMode(void);
static void SystemHeartbeat(void);

/**
 * Main entry point for the intercom firmware
 */
int main(void) {
    // Display startup message
    DisplayMessage("Initializing IC-2000X Two-way Intercom System");
    DisplayMessage("Firmware v" FIRMWARE_VERSION);
    DisplayMessage("Created by Ashwanth Kumar");
    
    // Initialize system components
    InitializeHardware();
    ConfigureNetwork();
    SetupAudioSubsystem();
    InitializeSecurityModule();
    
    // Perform startup diagnostics
    StartupSelfTest();
    
    // Register device with central monitoring server
    RegisterWithServer();
    
    // Main operation loop
    DisplayMessage("System Ready");
    
    while (1) {
        // Check for incoming calls
        CallInfo_t incomingCall;
        if (CheckForIncomingCall(&incomingCall)) {
            HandleIncomingCall(&incomingCall);
        }
        
        // Process any user input
        UserInput_t userInput;
        if (GetUserInput(&userInput)) {
            ProcessUserInput(userInput);
        }
        
        // Perform periodic tasks
        uptime++;
        if (uptime % HEARTBEAT_INTERVAL == 0) {
            SystemHeartbeat();
        }
        
        // Run diagnostics every 24 hours
        if (uptime % (86400) == 0) {
            PerformDiagnostics(DIAGNOSTIC_LEVEL_FULL);
        }
        
        // Check for emergency conditions
        if (CheckEmergencyCondition() && !emergencyModeActive) {
            EnterEmergencyMode();
        }
        
        // Sleep to save power
        Sleep(100); // 100ms sleep
    }
    
    // This should never be reached
    return 0;
}

/**
 * Initialize hardware components
 */
static void InitializeHardware(void) {
    DisplayMessage("Initializing hardware components...");
    
    // Initialize microcontroller peripherals
    InitializeMCU();
    
    // Configure GPIO pins
    SetupGPIO();
    
    // Initialize audio codec
    InitializeAudioCodec();
    
    // Setup display
    InitializeDisplay();
    
    // Initialize keypad
    InitializeKeypad();
    
    // Setup communication interfaces
    InitializeUART();
    InitializeSPI();
    InitializeI2C();
    
    // Initialize network hardware
    InitializeEthernet();
    InitializePSTNInterface();
    
    DisplayMessage("Hardware initialization complete");
}

/**
 * Configure network settings
 */
static void ConfigureNetwork(void) {
    DisplayMessage("Configuring network...");
    
    // Set default network configuration
    networkConfig.ipAddress = StringToIP(DEFAULT_IP_ADDRESS);
    networkConfig.subnetMask = StringToIP(DEFAULT_SUBNET_MASK);
    networkConfig.gateway = StringToIP(DEFAULT_GATEWAY);
    networkConfig.primaryDNS = StringToIP(PRIMARY_DNS);
    networkConfig.secondaryDNS = StringToIP(SECONDARY_DNS);
    
    // Try to get network configuration from DHCP
    if (GetNetworkConfigFromDHCP(&networkConfig)) {
        DisplayMessage("DHCP configuration successful");
    } else {
        DisplayMessage("Using default network configuration");
    }
    
    // Apply network configuration
    ApplyNetworkConfiguration(&networkConfig);
    
    // Test network connectivity
    if (TestNetworkConnectivity()) {
        DisplayMessage("Network connection established");
        char ipBuffer[16];
        IPToString(networkConfig.ipAddress, ipBuffer);
        DisplayMessage("IP Address: %s", ipBuffer);
    } else {
        DisplayMessage("WARNING: Network connection failed");
    }
}

/**
 * Setup audio processing subsystem
 */
static void SetupAudioSubsystem(void) {
    DisplayMessage("Configuring audio subsystem...");
    
    // Configure audio settings
    audioSettings.sampleRate = SAMPLE_RATE;
    audioSettings.bitDepth = BIT_DEPTH;
    audioSettings.channels = CHANNELS;
    audioSettings.noiseReductionLevel = NOISE_REDUCTION_LEVEL;
    audioSettings.echoCancellationLevel = ECHO_CANCELLATION_LEVEL;
    audioSettings.gainControlLevel = GAIN_CONTROL_LEVEL;
    
    // Apply audio settings
    ConfigureAudioProcessor(&audioSettings);
    
    // Test audio subsystem
    if (TestAudioSubsystem()) {
        DisplayMessage("Audio subsystem initialized successfully");
    } else {
        DisplayMessage("WARNING: Audio subsystem initialization failed");
    }
}

/**
 * Initialize security module
 */
static void InitializeSecurityModule(void) {
    DisplayMessage("Initializing security module...");
    
    // Configure security settings
    securitySettings.encryptionEnabled = true;
    securitySettings.encryptionLevel = ENCRYPTION_LEVEL_AES256;
    securitySettings.passwordProtectionEnabled = true;
    securitySettings.failedAttemptsThreshold = 3;
    securitySettings.lockoutDuration = 300; // 5 minutes
    
    // Initialize security module with settings
    InitializeSecurity(&securitySettings);
    
    // Generate and store encryption keys
    GenerateEncryptionKeys();
    
    DisplayMessage("Security module initialized");
}

/**
 * Perform startup self-test
 */
static void StartupSelfTest(void) {
    DisplayMessage("Performing system self-test...");
    
    // Test memory
    if (!TestMemory()) {
        DisplayMessage("WARNING: Memory test failed");
    }
    
    // Test storage
    if (!TestStorage()) {
        DisplayMessage("WARNING: Storage test failed");
    }
    
    // Test communication interfaces
    if (!TestCommunicationInterfaces()) {
        DisplayMessage("WARNING: Communication interface test failed");
    }
    
    // Test audio loopback
    if (!TestAudioLoopback()) {
        DisplayMessage("WARNING: Audio loopback test failed");
    }
    
    // Test power management
    if (!TestPowerManagement()) {
        DisplayMessage("WARNING: Power management test failed");
    }
    
    DisplayMessage("Self-test completed");
}

/**
 * Register device with central monitoring server
 */
static void RegisterWithServer(void) {
    DisplayMessage("Registering with central server...");
    
    // Prepare registration data
    RegistrationData_t regData;
    strncpy(regData.serialNumber, deviceSerialNumber, sizeof(regData.serialNumber));
    strncpy(regData.firmwareVersion, FIRMWARE_VERSION, sizeof(regData.firmwareVersion));
    strncpy(regData.model, DEVICE_MODEL, sizeof(regData.model));
    IPToString(networkConfig.ipAddress, regData.ipAddress);
    
    // Try primary server first
    bool registered = false;
    DisplayMessage("Connecting to primary server: %s", PRIMARY_SERVER);
    
    if (ConnectToServer(PRIMARY_SERVER, SERVER_PORT)) {
        if (SendRegistrationData(&regData)) {
            DisplayMessage("Registration successful with primary server");
            registered = true;
        }
    }
    
    // Try backup server if primary failed
    if (!registered) {
        DisplayMessage("Connecting to backup server: %s", BACKUP_SERVER);
        if (ConnectToServer(BACKUP_SERVER, SERVER_PORT)) {
            if (SendRegistrationData(&regData)) {
                DisplayMessage("Registration successful with backup server");
                registered = true;
            }
        }
    }
    
    if (!registered) {
        DisplayMessage("WARNING: Server registration failed");
    }
}

/**
 * Handle incoming call
 */
static void HandleIncomingCall(CallInfo_t *callInfo) {
    char buffer[64];
    
    // Log call information
    snprintf(buffer, sizeof(buffer), "Incoming call from: %s", callInfo->callerID);
    LogEvent(LOG_LEVEL_INFO, buffer);
    
    // Display call information
    DisplayMessage("Incoming Call: %s", callInfo->callerID);
    
    // Play ring tone
    PlayRingTone();
    
    // Wait for user to answer or call to timeout
    bool callAnswered = WaitForCallAnswer(30000); // 30 second timeout
    
    if (callAnswered) {
        // Stop ring tone
        StopRingTone();
        
        // Establish audio connection
        EstablishAudioConnection(callInfo->connectionID);
        
        // Process call until terminated
        ProcessActiveCall(callInfo->connectionID);
        
        // Call ended
        DisplayMessage("Call ended");
    } else {
        // Call not answered
        StopRingTone();
        DisplayMessage("Call not answered");
        
        // Record missed call
        RecordMissedCall(callInfo);
    }
}

/**
 * Process user input
 */
static void ProcessUserInput(UserInput_t input) {
    switch (input.type) {
        case INPUT_TYPE_KEYPAD:
            HandleKeypadInput(input.data.keyCode);
            break;
            
        case INPUT_TYPE_FUNCTION_BUTTON:
            HandleFunctionButton(input.data.functionCode);
            break;
            
        case INPUT_TYPE_VOLUME:
            AdjustVolume(input.data.volumeChange);
            break;
            
        case INPUT_TYPE_EMERGENCY:
            EnterEmergencyMode();
            break;
            
        case INPUT_TYPE_CONFIGURATION:
            EnterConfigurationMode();
            break;
            
        default:
            LogEvent(LOG_LEVEL_WARNING, "Unknown input type received");
            break;
    }
}

/**
 * Perform system diagnostics
 */
static void PerformDiagnostics(DiagnosticLevel_t level) {
    DisplayMessage("Performing diagnostics...");
    
    DiagnosticResults_t results;
    RunDiagnostics(level, &results);
    
    // Log diagnostic results
    LogDiagnosticResults(&results);
    
    // Check for critical issues
    if (results.criticalIssuesFound) {
        DisplayMessage("WARNING: Critical issues found during diagnostics");
        
        // Report issues to server
        ReportDiagnosticIssues(&results);
    } else {
        DisplayMessage("Diagnostics completed successfully");
    }
}

/**
 * Enter emergency mode
 */
static void EnterEmergencyMode(void) {
    DisplayMessage("EMERGENCY MODE ACTIVATED");
    
    // Set emergency flag
    emergencyModeActive = true;
    
    // Sound alarm
    ActivateEmergencyAlarm();
    
    // Establish emergency call
    CallInfo_t emergencyCall;
    strncpy(emergencyCall.callerID, EMERGENCY_NUMBER, sizeof(emergencyCall.callerID));
    emergencyCall.callType = CALL_TYPE_EMERGENCY;
    
    // Place emergency call
    if (PlaceCall(&emergencyCall)) {
        // Emergency call established
        ProcessActiveCall(emergencyCall.connectionID);
    } else {
        // Failed to establish emergency call
        DisplayMessage("Failed to establish emergency call");
    }
    
    // Deactivate emergency mode
    emergencyModeActive = false;
    DeactivateEmergencyAlarm();
    DisplayMessage("Emergency mode deactivated");
}

/**
 * System heartbeat function
 */
static void SystemHeartbeat(void) {
    // Prepare heartbeat data
    HeartbeatData_t heartbeatData;
    heartbeatData.uptime = uptime;
    heartbeatData.batteryLevel = GetBatteryLevel();
    heartbeatData.signalStrength = GetSignalStrength();
    heartbeatData.memoryUsage = GetMemoryUsage();
    heartbeatData.temperature = GetSystemTemperature();
    
    // Send heartbeat to server
    SendHeartbeat(&heartbeatData);
    
    // Log heartbeat
    LogHeartbeat(&heartbeatData);
}

// End of firmware
// Created by Ashwanth 

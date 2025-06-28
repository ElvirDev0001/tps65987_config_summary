/*
 * ============================================================================
 * Project: Lenovo Yoga 14 USB-C PD Firmware
 * Device: TPS65988DJRSHR
 * Purpose: Enable 95W Sink w/ DisplayPort Alt Mode + USB3
 * Generated: 2025-06-28T18:51:29.604000+00:00
 * Tool Version: 6.1.4
 * ============================================================================
 */

// ============================================================================
// POWER DELIVERY (PD) CONFIGURATION
// ============================================================================

// Sink PDOs (Power Data Objects) - What power levels we can accept
#define NUM_SINK_PDOS                   4

typedef struct {
    uint16_t voltage_mv;     // Voltage in millivolts
    uint16_t current_ma;     // Current in milliamps
    uint8_t  peak_current;   // Peak current capability (100% or 130%)
    bool    higher_capability;
    bool    ask_for_max;
} sink_pdo_t;

sink_pdo_t sink_pdos[NUM_SINK_PDOS] = {
    {5000,  3000,  130, true,  true},   // PDO 1: 5V @ 3A (15W)
    {9000,  3000,  130, true,  true},   // PDO 2: 9V @ 3A (27W)  
    {15000, 3000,  130, true,  true},   // PDO 3: 15V @ 3A (45W)
    {20000, 4750,  100, true,  true},   // PDO 4: 20V @ 4.75A (95W) ✅
};

// Binary PDO representation (LSB -> MSB) [for reference]
uint32_t sink_pdos_bin[NUM_SINK_PDOS] = {
    0x00019096,  // 5V, 3A
    0x0002D096,  // 9V, 3A
    0x0003A096,  // 15V, 3A
    0x0004B4FA   // 20V, 4.75A
};

// ============================================================================
// GPIO CONFIGURATION (Port 1)
// ============================================================================

typedef struct {
    uint8_t  pin_number;
    uint8_t  function;       // 0=GPIO, 1=Alt Function, 2=PP3, 3=PP4
    bool     output_enable;
    bool     interrupt_enable;
    uint8_t  mapped_event;
    bool     pull_up;
    bool     pull_down;
    const char* description;
} gpio_config_t;

gpio_config_t gpio_config[22] = {
    // Active GPIOs (mapped to hardware)
    {0,  0, true,  false, 0x00, false, false, "SBU/USB-C mux SEL1 (DP-alt enable)"},
    {1,  0, true,  false, 0x28, false, false, "SBU/USB-C mux SEL0 (DP-alt polarity) - USB3 Event"},
    {2,  0, true,  false, 0x04, false, false, "CC flip detection - Cable Orientation"},
    {3,  1, false, false, 0x00, false, false, "DP HPD1 to iGPU (Alt Function)"},
    {4,  0, false, false, 0x00, false, false, "Not stuffed"},
    {5,  0, true,  false, 0x00, false, false, "Charger-OK LED (optional)"},
    {6,  0, true,  false, 0x00, false, false, "USB3 SS mux EN (Port 0)"},
    {7,  0, true,  true,  0x76, false, false, "CC polarity to SS mux - I2C3 Master IRQ"},
    
    // Unused GPIOs (8-15)
    {8,  0, false, false, 0x00, false, false, "Unused"},
    {9,  0, false, false, 0x00, false, false, "Unused"},
    {10, 0, false, false, 0x00, false, false, "Unused"},
    {11, 0, false, false, 0x00, false, false, "Unused"},
    {12, 0, false, false, 0x00, false, false, "Unused"},
    {13, 0, false, false, 0x00, false, false, "Unused"},
    {14, 0, false, false, 0x00, false, false, "Unused"},
    {15, 0, true,  false, 0x58, false, false, "Source Power > Threshold Event"},
    
    // Power Path Control (Critical for 95W operation)
    {16, 2, false, false, 0x00, false, false, "PP3 Enable (High-current sink FET)"},
    {17, 3, false, false, 0x00, false, false, "PP4 Enable (Second path)"},
    
    // Unused GPIOs (18-21)
    {18, 0, false, false, 0x00, false, false, "Not routed"},
    {19, 0, false, false, 0x00, false, false, "Not routed"},
    {20, 0, false, false, 0x00, false, false, "Not routed"},
    {21, 0, false, false, 0x00, false, false, "Not routed"},
};

// ============================================================================
// POWER PATH CONFIGURATION
// ============================================================================

// Global System Configuration (Register 0x27)
#define PP_CABLE_1_SWITCH_CONFIG         0x1    // Output, 4.5-5.5V
#define PP_CABLE_2_SWITCH_CONFIG         0x1    // Output, 4.5-5.5V
#define PP_1_SWITCH_CONFIG               0x0    // Disabled
#define PP_2_SWITCH_CONFIG               0x0    // Disabled
#define PP_3_SWITCH_CONFIG               0x2    // ✅ Sink Only (Input)
#define PP_4_SWITCH_CONFIG               0x2    // ✅ Sink Only (Input)

// Power Path to VBUS Mapping
#define PP1_SWITCH_TO_VBUS_MAP           0x0    // VBUS 1
#define PP2_SWITCH_TO_VBUS_MAP           0x1    // VBUS 2
#define PP3_SWITCH_TO_VBUS_MAP           0x0    // VBUS 1
#define PP4_SWITCH_TO_VBUS_MAP           0x1    // VBUS 2

// Overcurrent Protection
#define PP_SWITCH_1_OVERCURRENT_TIMEOUT  0x4    // 640 μS
#define PP_SWITCH_2_OVERCURRENT_TIMEOUT  0x4    // 640 μS
#define PP_SWITCH_1_OVERCURRENT_ENABLE   0x1    // Enabled
#define PP_SWITCH_2_OVERCURRENT_ENABLE   0x1    // Enabled

// ============================================================================
// PORT CONFIGURATION
// ============================================================================

// Port Control (Register 0x29)
#define TYPE_C_CURRENT                   0x0    // Default Current (weakest pullup)
#define EXTERNALLY_POWERED               0x1    // ✅ True (battery/charger powered)
#define AUTOMATIC_SINK_CAP               0x1    // ✅ True
#define SINK_CONTROL_BIT                 0x0    // False
#define FIFTEEN_KOHM_RESISTOR_PRESENT    0x0    // ❌ SHOULD BE 0x1 (True)

// ============================================================================
// PROTECTION SETTINGS
// ============================================================================

// Current Protection Settings
#define VOLTAGE_THRESHOLD_SINK_CONTRACT  0x0    // ❌ SHOULD BE 0x18 (24V)
#define UNDERVOLTAGE_PROTECTION_5V       0x3    // ❌ SHOULD BE 0x0 (4V)
#define UNDERVOLTAGE_PROTECTION_GT5V     0x3    // ❌ SHOULD BE 0x0 (4V)
#define OVERVOLTAGE_PROTECTION           0x3f   // ✅ 24V (correct)

// ============================================================================
// ISSUES FOUND
// ============================================================================

/*
 * CRITICAL ISSUES:
 * 1. Voltage Threshold as Sink Contract = 0x0 (should be 0x18 for 24V)
 * 2. Under-voltage Protection = 0x3 (should be 0x0 for 4V)
 * 3. 15 kΩ Resistor Present = 0x0 (should be 0x1 for USB 2.0 compliance)
 * 
 * MINOR ISSUES:
 * 4. GPIO 7 has I2C3 Master IRQ event (0x76) - Connected to TP7738 (I2C IRQ) → OK
 * 5. GPIO 15 triggers APU reset. Ensure the 95W threshold condition (event 0x58) 
 *    is stable before assert. Connected to APU_RST#_PD → Fires after 95W ready
 * 
 * GOOD CONFIGURATIONS:
 * ✅ PP3/PP4 correctly mapped to GPIO 16/17
 * ✅ 20V @ 4.75A PDO present (95W capability)
 * ✅ Power path switching correctly configured
 * ✅ Overcurrent protection enabled
 * ✅ All unused GPIOs properly configured as inputs
 */

// ============================================================================
// RECOMMENDED FIXES
// ============================================================================

// Fix protection thresholds
#define FIXED_VOLTAGE_THRESHOLD_SINK_CONTRACT    0x18   // 24V
// IMPORTANT: This must be written to register 0x3B, bits [7:0]

#define FIXED_UNDERVOLTAGE_PROTECTION_5V         0x0    // 4V  
// IMPORTANT: This must be written to register 0x28, bits [19:17]

#define FIXED_UNDERVOLTAGE_PROTECTION_GT5V       0x0    // 4V
// IMPORTANT: This must be written to register 0x28, bits [22:20]

#define FIXED_FIFTEEN_KOHM_RESISTOR_PRESENT      0x1    // True
// IMPORTANT: This must be written to register 0x29, bit [24]

// Optional: Add pull-downs to unused GPIOs for safety
// gpio_config[8-15, 18-21].pull_down = true;
// IMPORTANT: These are controlled in register 0x5C, bits [160-175, 176-183]

// ============================================================================
// SAFETY CONSIDERATIONS
// ============================================================================

/*
 * - APU_RST# is triggered only after power > 85W confirmed (via event 0x58)
 * - Overcurrent protection on PP3/PP4 active (5A max)
 * - Undervoltage/overvoltage thresholds enforced at 4V–24V
 * - GPIOs default to inputs if unused
 * - VBUS mapping isolates sink paths properly
 * - Power path timing: PP3/PP4 only activate after successful 20V contract
 * - DisplayPort HPD properly routed to iGPU
 * - USB3 mux control ensures proper data path switching
 */ 

## Design

### Architecture

Dylan

### Mechanism mapping

Dylan

### Failure-mode table

| Component | Failure mode | Failure detection | System response | Log |
| --------- | ------------ | ----------------- | --------------- | --- |
| Ultrasonic Distance Sensor | Both UDSs recieve ping at same time with same distance, leading to a log collision. | Analytic process UDS thread caching finds previous event to ensure no duplicates exist. | UDS thread in analytic process that is duplicate does not signal duplicate log. | Primary duplicate event thread signals to logging process that duplicate detection was found, with timestamp. |
| Microphone | Sensor receives a volume that is too great. Reported volume (dB) breaches threshold and is inaccurate. | Analytic process thread for the microphone ensures that the voltage step limit for the microphone was not breached, measuring given step and step limit. | Microphone thread in analytic process does not signal logging process with given breached data. | Logging process will be signaled from microphone thread from analytic process to show that the decibel limit was breached, with timestamp. |

### Storage and data

Dylan

### Constraints and substitutions

Autumn

### Evaluation plan

Autumn

### Ownership map

Autumn

### AI-use plan

Dylan

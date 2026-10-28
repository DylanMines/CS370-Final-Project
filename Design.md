## Design

### Architecture

![Diagram](https://github.com/DylanMines/CS370-Final-Project/blob/main/Archetecture.drawio.png)

### Mechanism mapping

Mechanism B: Interrupt driven input, with a head to head comparison against a polling design. Implemented by the keypad and interface processes. The interrupt here is important for the user experience. Without it, inputs could be dropped. If a process sits idle constantly checking for inputs in a loop, it wastes CPU time and can miss the input entirely if the human pressing the button is faster than the polling time. This is why we will use GPIO interrupts with our keypad. One pin for each row and column. Rows are inputs, columns are outputs. Outputs are set to low, inputs are connected to an internal pullup resistor. When a button is pressed, the pi detects the change on the input and scans to see which column corresponds to the pressed row. This triggers a function that allows an action to be performed based on the button pressed. 7 pins total, 4 interrupts used.

Mechanism D: A custom storage layer: ring buffer, append-only log. The logger process is able to record events happening while being disconnected from those actual events. If the sampling process dies, the logger persists. If there is a critical bug in the interface, doesn't matter. The logger only adds and reads the record, nothing else. The ring buffers from the sampler give the logger wiggle room with timing, while ensuring timestamps are correct. 

Mechanism F: A high-rate sampling pipeline with a no-drop guarantee: lock-free or single-producer/single-consumer ring between capture and consumption. Each sensor gets its own thread in the sampling process AND the logging process. A single bounded buffer exists between each thread, ensuring the logger will always see anything the sampler collects. When a thread decides that it is it's turn to sample, it can complete a full sampling (out and back for ultrasonic, analog input for microphone). Each sensor has a maximum sampling rate the sampling process can coordinate.

Mechanism E: A multi-process architecture: sensors isolated in separate processes, a hub aggregating over IPC (shared memory, message queues, or Unix domain sock- ets), a supervisor that survives any child dying. Each process has its own responsibility, and doesn't touch anything else. The sampler never compares or processes, just passes things happily along to the logger. The logger doesn't touch I/O or try to compare entries, just writes them down. The analytics engine doesn't write to files or touch I/O. All of these places are possible failure points with only one process affected. Supervisor process is able to restart and connect IPC again, but doesn't take over anyone else's job in while doing so.

### Failure-mode table

| Component | Failure mode | Failure detection | System response | Log |
| --------- | ------------ | ----------------- | --------------- | --- |
| Ultrasonic Distance Sensor | Both UDSs recieve ping at same time with same distance, leading to a log collision. | Analytic process UDS thread caching finds previous event to ensure no duplicates exist. | UDS thread in analytic process that is duplicate does not signal duplicate log. | Primary duplicate event thread signals to logging process that duplicate detection was found, with timestamp. |
| Microphone | Sensor receives a volume that is too great. Reported volume (dB) breaches threshold and is inaccurate. | Analytic process thread for the microphone ensures that the voltage step limit for the microphone was not breached, measuring given step and step limit. | Microphone thread in analytic process does not signal logging process with given breached data. | Logging process will be signaled from microphone thread from analytic process to show that the decibel limit was breached, with timestamp. |

### Storage and data

We'll store both raw data from sensors and events from the analytics engine. 
Raw data takes the form of a single line entry, with `timestamp`, `source`, `value`. Timestamp is when the data is generated, not read. Source indicates which sensor this is from (Ultrasonic A, Ultrasonic B, Microphone). Keypad doesn't deal with the sampler or the logger so it does not need to be written down. This is written at whatever the sampling rate of the sensors is. At the maximum values, 40,000 entries per second per sensor. That's too much. The sampling rate of sensors will likely be much slower, in the <=100 HZ range. 
Event data takes a similar form. `timestamp`, `source`, and `event`. Timestamp is the same as before. Source indicates which sensors were used in the decision an event happened. Ultrasonic sensors found something the microphone didnt? Source will read `Ultrasonic A, Ultrasonic B`. This creates a trace to verify sensors are working together, and what they find. Event describes the event. Person entered the room, person left the room, noise heard inside house, door opened and person entered. This is the inferred context of the real world based on the sensor input. 
Records are permanent, written on disk, and sequential. When a new record is written, it is stored up with other records. As soon as a blocks worth of records is collected, write them too disk, start over. Should power fail mid-write, everything up too that point is safely on disk already. The block that is being written to is thrown out, only a few individual records lost. At the sampling rate, this shouldn't be an issue.

### Constraints and substitutions

Autumn

### Evaluation plan

Autumn

### Ownership map

Autumn

### AI-use plan

We'll use claude code to help us write individual modules and to iterate with. System architecture, sampling rates, libraries, language, and kernel/user boundary remain squarely in our human hands. The architecture flow chart and mechanisms will stay as high-level descriptors. We might feed them into claude and iterate on strategy, but nothing will be written until the owner of a particular sub-system approves it. Logic processing is done entirely on-board. If the program can't run without an internet connected, it might as well not run at all. Individual functions, modules, data structures will be outsourced to claude. We'll give it a scope, a goal, how it fits into design, specific libraries, and read the plan. Only once approved to we allow claude to write, and we will not ignore tests along the way.

# Dtests

Testing Dyalog APL Code using a custom test framework.

## Usage
### Using RIDE:
Open ride in the repository
Link the namespaces:

```
]LINK.Create # .
```
Run the test cases
```
unittest.RunTests [test_namespace] [verbose= 1|0] [stop=1|0] [⎕RL=any seed value(''?'' for random)] (0 default)
```
verbose: if set to 0, only output failing tests and a single summary line.

stop: if set to 1, any test which fails causes the framework to stop and allows the developer to inspect the failing test.

⎕RL: Seed value to for the random link variable that generates random numbers through the tests (it gets reset after each test)

Example:
```
unittest.RunTests tests.test_membership 1 0 1232
```
or
```
unittest.RunTests tests.test_membership
```

<!-- ## Configuration

#### APL information:

- Jupyter kernel: https://github.com/Dyalog/dyalog-jupyter-kernel using commit: 421dd512207daaf3aa62589839fac0edd9365c54
- Dyalog version: Dyalog APL/S-64 Version 18.2.45405

#### System information:
```
❯ neofetch
██████████████████  ████████   rush@rush 
██████████████████  ████████   --------- 
██████████████████  ████████   OS: Manjaro Linux x86_64 
██████████████████  ████████   Host: TUF Gaming FX505DT_FX505DT 1.0 
████████            ████████   Kernel: 5.15.85-1-MANJARO 
████████  ████████  ████████   Uptime: 3 days, 14 mins 
████████  ████████  ████████   Packages: 1684 (pacman), 101 (nix-user), 43 (nix-default) 
████████  ████████  ████████   Shell: zsh 5.9 
████████  ████████  ████████   Resolution: 1920x1080 
████████  ████████  ████████   WM: i3 
████████  ████████  ████████   Theme: Breeze [GTK2/3] 
████████  ████████  ████████   Icons: breeze [GTK2/3] 
████████  ████████  ████████   Terminal: konsole 
████████  ████████  ████████   CPU: AMD Ryzen 7 3750H with Radeon Vega Mobile Gfx (8) @ 2.300GHz 
                               GPU: NVIDIA GeForce GTX 1650 Mobile / Max-Q 
                               GPU: AMD ATI Radeon Vega Series / Radeon Vega Mobile Series 
                               Memory: 5497MiB / 15807MiB                        
``` -->
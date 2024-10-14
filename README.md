## Digital Integrated Circuitd Working Group
----

Do the following:

1) Run "nano ~/.bashrc"

2) Scroll to the bottom and paste the code block appened to the below to your bashrc

3) Press CTRL-O, then yes

4) Press CTRL-X, then yes, then enter

5) Run "source ~/.bashrc"

6) Now you can start Cadence appropriately by simply typing "cadence" in terminal (no need for tcsh).

---

```
cadence() {
    # Change to the Dropbox directory
    cd ~/esc-courses/fa24-cse-30342.01/dropbox/ || return

    # Check if the cadence_nd.sh file exists
    if [ ! -f "cadence_nd.sh" ]; then
        # Download the script if it does not exist
        wget https://raw.githubusercontent.com/mmorri22/cse30342/main/Resources/cadence_nd.sh
    fi

    # Run tcsh and execute the commands within tcsh
    tcsh -c "
        source cadence_nd.sh
        if (\$status != 0) then
            exit 1
        endif

        cd VLSI
        if (\$status != 0) then
            exit 1
        endif

        if (-d digital-integrated-circuits) then
            cd digital-integrated-circuits
            if (\$status != 0) then
                exit 1
            endif

            git pull
            if (\$status != 0) then
                echo \"Merge conflict or pull failed. Please resolve manually.\"
            endif
        else
            git clone https://github.com/starlight-traveler/digital-integrated-circuits.git
            if (\$status != 0) then
                exit 1
            endif

            cd digital-integrated-circuits
            if (\$status != 0) then
                exit 1
            endif
        endif

        virtuoso -64 &
    "
}
```

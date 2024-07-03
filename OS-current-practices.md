   # OS current practices
   
   Most modern OSes (especially mobile ones) do implement by default some MAC address randomization policy. Table 1 summarizes current practices for Androiod and iOS, as the time of writing this document
   (original source posted at: [Private MAC address on iOS 14](https://www.fing.com/news/private-mac-address-on-ios-14), latest wayback machine's snapshot available [here](https://web.archive.org/web/20230905111429/https://www.fing.com/news/private-mac-address-on-ios-14), updated based on findings from the authors of [draft-ietf-madinas-mac-address-randomization](https://datatracker.ietf.org/doc/draft-ietf-madinas-mac-address-randomization/).

    +=============================================+===================+
    | Android 10+                                 | iOS 14+           |
    +=============================================+===================+
    | The randomized MAC address is bound to the  | The randomized    |
    | SSID                                        | MAC address is    |
    |                                             | bound to the      |
    |                                             | BSSID             |
    +---------------------------------------------+-------------------+
    +---------------------------------------------+-------------------+
    | The randomized MAC address is stable across | The randomized    |
    | reconnections for the same network          | MAC address is    |
    |                                             | stable across     |
    |                                             | reconnections for |
    |                                             | the same network  |
    +---------------------------------------------+-------------------+
    +---------------------------------------------+-------------------+
    | The randomized MAC address does not get re- | The randomized    |
    | randomized when the device forgets a WiFI   | MAC address is    |
    | network                                     | reset when the    |
    |                                             | device forgets a  |
    |                                             | WiFI network      |
    +---------------------------------------------+-------------------+
    +---------------------------------------------+-------------------+
    | MAC address randomization is enabled by     | MAC address       |
    | default for all the new WiFi networks.  But | randomization is  |
    | if the device previously connected to a     | enabled by        |
    | WiFi network identifying itself with the    | default for all   |
    | real MAC address, no randomized MAC address | the new WiFi      |
    | will be used (unless manually enabled)      | networks          |
    +---------------------------------------------+-------------------+

        Table 1: Android and iOS MAC address randomization practices

   In September 2021, we have performed some additional tests to
   evaluate how most widely used OSes behave regarding MAC address
   randomization.  Table 2 summarizes our findings, where show on
   different rows whether the OS performs address randomization per
   network (PNGM according to the taxonomy introduced in Section 6 of
   [draft-ietf-madinas-mac-address-randomization](https://datatracker.ietf.org/doc/draft-ietf-madinas-mac-address-randomization/).), per
   new connection (PSGM), daily (PPGM with a period of 24h), supports
   configuration per SSID, supports address randomization for scanning,
   and whether it does that by default.

      +=================+=======+============+============+=========+
      | OS              | Linux | Android 10 | Windows 10 | iOS 14+ |
      +=================+=======+============+============+=========+
      | Random per net. |   Y   |     Y      |     Y      |    Y    |
      | (PNGM)          |       |            |            |         |
      +-----------------+-------+------------+------------+---------+
      +-----------------+-------+------------+------------+---------+
      | Random per      |   Y   |     N      |     N      |    N    |
      | connec.  (PSGM) |       |            |            |         |
      +-----------------+-------+------------+------------+---------+
      +-----------------+-------+------------+------------+---------+
      | Random daily    |   N   |     N      |     Y      |    N    |
      | (PPGM)          |       |            |            |         |
      +-----------------+-------+------------+------------+---------+
      +-----------------+-------+------------+------------+---------+
      | SSID config.    |   Y   |     N      |     N      |    N    |
      +-----------------+-------+------------+------------+---------+
      +-----------------+-------+------------+------------+---------+
      | Random. for     |   Y   |     Y      |     Y      |    Y    |
      | scan            |       |            |            |         |
      +-----------------+-------+------------+------------+---------+
      +-----------------+-------+------------+------------+---------+
      | Random. for     |   N   |     Y      |     N      |    Y    |
      | scan by default |       |            |            |         |
      +-----------------+-------+------------+------------+---------+
 
      Table 2: Observed behavior from different OS (as of September
                                  2022)

   According to ["MAC Randomization Behavior"](https://source.android.com/devices/tech/connect/wifi-mac-randomization-behavior), starting in Android 12, Android uses non-persistent randomization in the following situations: (i) a
   network suggestion app specifies that non-persistant randomization be used for the network (through an API); or (ii) the network is an open network that hasn't encountered a captive portal and an internal
   config option is set to do so (by default it is not).

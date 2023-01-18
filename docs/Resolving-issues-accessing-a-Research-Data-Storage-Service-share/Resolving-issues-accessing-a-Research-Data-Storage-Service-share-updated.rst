Resolving issues accessing a Research Data Storage Service share
================================================================

If you are off-campus, you must use the Northwestern VPN to connect to a
Research Data Storage Service (RDSS) share. If you are on-campus, you
must use Northwestern’s (or your school’s) DNS servers in order to
connect successfully.

.. container:: expander expander1

   .. rubric:: Configure DNS to use Northwestern DNS servers
      :name: configure-dns-to-use-northwestern-dns-servers

   .. container::

      If you are on-campus and having difficulties connecting to an RDSS
      share, you may need to configure your DNS settings to use
      Northwestern’s DNS servers. The firewall settings for RDSS also
      allow you to use your school’s DNS settings. If you are interested
      in this latter option, contact your local IT department for more
      information on their DNS settings.

      .. container:: expander expander2

         .. rubric:: Configuring DNS on Windows
            :name: configuring-dns-on-windows

         .. container::

            #. Open the **Start Menu** and select **Control Panel**.
            #. In the search box at the top right corner, search
               **Network and Sharing Center** and click on the result.
            #. From the menu on the left, select **Change adapter
               settings**.
            #. Select\ **Local Area Connection**\ or **Ethernet**, and
               select\ **Properties**\ from the pop up menu.
            #. Select **Internet Protocol Version 4 (TCP/IPv4)**, then
               click **Properties**.
            #. Check **Use the following DNS server addresses**.
            #. In **Preferred DNS server**, enter **129.105.49.1**
            #. In **Alternate DNS server**, enter **165.124.49.21**
            #. Click **OK** to close the **Internet Protocol (TCP/IPv4)
               Properties** window.
            #. Click **OK** to close the **Local Area Connection
               Properties** window.

         .. rubric:: Configuring DNS on macOS
            :name: configuring-dns-on-macos

         .. container::

            #. In the top left corner, click the **Apple** icon.
            #. Click **System Preferences**.
            #. Click **Network**.
            #. Click on the network you are using, such as Thunderbolt
               Port, Ethernet, or Wifi.
            #. Click **Advanced**.
            #. Click the **DNS** tab.
            #. In the bottom left corner, under the **DNS Servers** box,
               click the **plus (+) sign**.
            #. Enter **129.105.49.1**
            #. Click the **plus (+) sign** again.
            #. Enter **165.124.49.21**
            #. Click **OK** to close this window.
            #. Click **Apply**.

   .. rubric:: Connect to Northwestern VPN
      :name: connect-to-northwestern-vpn

   .. container::

      If you are off campus, Northwestern VPN is required to connect to
      an RDSS share. It is not suggested to use VPN if you are
      on-campus.

      .. container:: expander expander2

         .. rubric:: Setting up VPN on Windows
            :name: setting-up-vpn-on-windows

         .. container::

            .. container::

               If your University-owned computer is managed by your
               department, you may not need to set up GlobalProtect. If
               you see the GlobalProtect icon
               (|globalprotect-Win-tray-icon.png|) in your system tray,
               skip the set-up instructions and go directly to `connect
               to GlobalProtect <#connect>`__.

               .. container:: expander expander1

                  .. rubric:: Set up GlobalProtect
                     :name: set-up-globalprotect

                  .. container::

                     Note that your computer must be running Windows 7
                     or later. If you don’t have administrative rights
                     to your University-owned computer to install
                     software, contact your departmental tech support
                     personnel.

                     #. Go to
                        `https://chocolate.ci.northwestern.edu\Software\Titles\NU_Global_Protect\Current\Windows\GlobalProtect64x.msi <https://chocolate.ci.northwestern.edu/Software/Titles/NU_Global_Protect/Current/Windows/GlobalProtect64x.msi>`__
                        to download the **GlobalProtect** installation
                        package.
                     #. From your computer’s **Downloads** folder,
                        double-click the installer, then click Next to
                        follow the installation instructions.
                     #. When prompted for a portal address, enter
                        **vpn-connect.northwestern.edu**.
                     #. Once installation is complete,

                        -  GlobalProtect will appear in the lower left
                           area of your system tray.
                           |GlobalProtectinWindowsSystemTray|
                        -  GlobalProtect will automatically prompt you
                           to connect to VPN.

                  .. rubric:: Connect to GlobalProtect
                     :name: connect-to-globalprotect

                  .. container::

                     #. Click the **GlobalProtect** icon in the system
                        tray, then click **Connect**. If nothing seems
                        to happen when you click **Connect**, see
                        `Fixing when clicking Connect in GlobalProtect
                        VPN for Windows does
                        nothing <page.php?id=99180>`__.
                        |Global Protect Windows connect|
                     #. When prompted, enter your NetID and NetID
                        password, then confirm your identity with Duo
                        multi-factor authentication. You will then be
                        connected to GlobalProtect.
                     #. To disconnect, click the **GlobalProtect** icon
                        again, then click **Disconnect**.
                        |Global Protect Windows disconnect|

         .. rubric:: Setting up VPN on macOS
            :name: setting-up-vpn-on-macos

         .. container::

            If your University-owned computer is managed by your
            department, you may not need to set up GlobalProtect. If you
            see the GlobalProtect icon
            (|globalprotect-Mac-menu-icon.png|) in your menu bar, skip
            the set-up instructions and go directly to `connect to
            GlobalProtect <#connect>`__.

            .. container:: expander expander1

               .. rubric:: Set up GlobalProtect
                  :name: set-up-globalprotect-1

               .. container::

                  Note that your Mac must be running macOS Big Sur
                  (11.X), Catalina (10.15), Mojave (10.14), High Sierra
                  (10.13), or Sierra (10.12).

                  #. Go to
                     `https://chocolate.ci.northwestern.edu\Software\Titles\NU_Global_Protect\Current\MacOS\GlobalProtect.pkg <https://chocolate.ci.northwestern.edu/Software/Titles/NU_Global_Protect/Current/MacOS/GlobalProtect.pkg>`__
                     to download the **GlobalProtect** installation
                     package.
                  #. Within your computer’s **Downloads** folder,
                     double-click the downloaded file to begin
                     installation. When prompted, click **Continue**,
                     then click **Install**.
                  #. If you see a prompt about a PaloAlto system
                     extension being blocked, click **Open Security
                     Preferences**.
                     |PaloAlto extension blocked|

                     a. Click **Allow**, then close your preferences
                        window.
                        |Blocked|
                     b. If prompted, enter your computer’s
                        administrative user name and password, then
                        click **Install Software**. If you don’t have
                        administrative rights to your University-owned
                        computer, contact your departmental tech support
                        personnel.
                     c. Restart your Mac.

                  #. When the installation finishes, you will be
                     prompted to move the installer the trash.
                     |Move installer to trash|
                  #. You will then be prompted asking for your keychain
                     password. Enter your password and choose **Always
                     Allow**.
                  #. Click **Close**. Once installation is complete,
                     GlobalProtect will appear in your menu bar at the
                     top of your macOS desktop.

               .. rubric:: Connect to GlobalProtect
                  :name: connect-to-globalprotect-1

               .. container::

                  #. Click the **GlobalProtect** icon in the menu bar,
                     enter portal address
                     **vpn-connect.northwestern.edu**, then click
                     **Connect**.
                     |Global Protect Not Connected|
                  #. When prompted, enter your NetID and NetID password,
                     then confirm your identity with Duo multi-factor
                     authentication. You will then be connected to
                     GlobalProtect.
                  #. To disconnect, click the **GlobalProtect** icon
                     again, then click **Disconnect**.
                     |GlobalProtect connected|

.. |globalprotect-Win-tray-icon.png| image:: https://kb.northwestern.edu/images/group293/62248/globalprotect-Win-tray-icon.png
   :class: fr-fic fr-dii
   :width: 15px
.. |GlobalProtectinWindowsSystemTray| image:: https://kb.northwestern.edu/images/group293/94726/GlobalProtectWin-SystemTray.png
   :class: fr-fic fr-dii
   :width: 125px
.. |Global Protect Windows connect| image:: https://kb.northwestern.edu/images/group293/94726/GlobalProtectWIndowsConnect.png
   :class: fr-fic fr-dii
   :width: 150px
.. |Global Protect Windows disconnect| image:: https://kb.northwestern.edu/images/group293/94726/GlobalProtectWindowsDisconnect.png
   :class: fr-fic fr-dii
   :width: 150px
.. |globalprotect-Mac-menu-icon.png| image:: https://kb.northwestern.edu/images/group293/62249/globalprotect-Mac-menu-icon.png
   :class: fr-fic fr-dii
   :width: 15px
.. |PaloAlto extension blocked| image:: https://kb.northwestern.edu/images/group293/94726/3GP-system-extension-blocked.png
   :class: fr-fic fr-dii
   :width: 300px
.. |Blocked| image:: https://kb.northwestern.edu/images/group293/94726/4GP-PaloAlto-is-blocked.png
   :class: fr-fic fr-dii
   :width: 400px
.. |Move installer to trash| image:: https://kb.northwestern.edu/images/group293/94726/6GP-move-installer-to-trash.png
   :class: fr-fic fr-dii
   :width: 300px
.. |Global Protect Not Connected| image:: https://kb.northwestern.edu/images/group293/62249/GlobalProtect-NotConnected-macOS.png
   :class: fr-fic fr-dii
   :width: 150px
.. |GlobalProtect connected| image:: https://kb.northwestern.edu/images/group293/62249/GlobalProtect-Connected-macOS.png
   :class: fr-fic fr-dii
   :width: 150px

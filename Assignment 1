from netmiko import (
    ConnectHandler,
    NetmikoTimeoutException,
    NetmikoAuthenticationException,
)
import sqlite3
import threading
import getpass
import time

def typeVerification(routerConfig,commands):
    #user input for connection type
    connectionType = input("Do you want a secure connection?(Y/N) ")

    #taking password and secret parameters from the user
    routerConfig["password"] = getpass.getpass("Input password: ")
    routerConfig["secret"] = getpass.getpass("Input secret: ")
    
    #ssh connection
    if connectionType.upper()=='Y':
        #setting device_type parameter for ssh
        routerConfig["device_type"] = 'cisco_ios'

        #connects to router
        sshRouterConnect = ConnectHandler(**routerConfig)
        #enable mode
        sshRouterConnect.enable()

        #execute command and store into variable
        config = sshRouterConnect.send_command(commands[-1], use_textfsm=True)
        configBackup(config)

    #telnet connection
    elif connectionType.upper()=='N':
        #setting device_type parameter for telnet
        routerConfig["device_type"] = 'cisco_ios_telnet'

        #connects to router
        telnetRouterConnect = ConnectHandler(**routerConfig)
        #enable mode
        telnetRouterConnect.enable()

        #execute command and store into variable
        config = telnetRouterConnect.send_command(commands[-1], use_textfsm=True)
        configBackup(config)

    #erroneous inputs
    else:
        print('ERROR: Invalid input')
        typeVerification(routerConfig,commands)

def configBackup(config):
    full = input('Do you want the full configuration of the Router written to a text file? (Y/N)')

    if full.upper() ==  'Y':
        #write permissions to the text file
        backup = open('config.txt','w+')
        #writing the config backup to the text file
        backup.write(config)

        print('Backup of Router Configuration has been created successfully!')

    elif full.upper() == 'N':
        print('EXITING THE PROGRAM')
    
    else:
        print('ERROR: Invalid input')
        configBackup(config)

    dbBackup()

def dbBackup():
    user = input('Do you want a database of detailed configuration? (Y/N)')

    if user.upper() == 'Y':
        db(tables,routerConfig['host'])

    elif user.upper() == 'N':
        print('EXITING THE PROGRAM')

    else:
        print('ERROR: Invalid input')
        dbBackup()
    

def db(tables,host):
    #creating the database
    con = sqlite3.connect(f'{host}.db')
    cursor = con.cursor()

    createTable(con,cursor,tables, host)


def createTable(con,cursor,tables,host):
    #create database table
        cursor.execute(f'CREATE TABLE {tables[0]} IF NOT EXIST;')
        print(f'Database created successfully!')

        fillDatabase(tables[1],routerConfig,con,cursor)

def fillDatabase(commands,routerConfig,con,cursor):
    commands.remove('sh run')
    sshRouterConnect = ConnectHandler(**routerConfig)
    sshRouterConnect.enable()

    for x in commands:
        if x == 0:
            config = sshRouterConnect.send_command(commands[x], use_textfsm=True)

        elif x == 1:
            config = sshRouterConnect.send_command(commands[x], use_textfsm=True)
        elif x == 2:
            config = sshRouterConnect.send_command(commands[x], use_textfsm=True)
        elif x == 3:
            config = sshRouterConnect.send_command(commands[x], use_textfsm=True)
        elif x == 4:
            config = sshRouterConnect.send_command(commands[x], use_textfsm=True)
        elif x == 5:
            config = sshRouterConnect.send_command(commands[x], use_textfsm=True)
        elif x == 6:
            config = sshRouterConnect.send_command(commands[x], use_textfsm=True)
        else:
            print('ERROR: Index Out of Range')


def interfacesBrief(con,cursor):
    #User input
    interfaces = input('Do you want to view interfaces information? (Y/N)')

    #Conditions
    if interfaces.upper() == 'Y':
        #read from DB
        print(cursor.execute('SELECT * FROM InterfacesBrief;'))
        #delay to read
        time.sleep(10)
        #user input to continue
        proceed = input('Do you want to continue? (Y/N)')

        if proceed.upper() == 'Y':
            vlans(con,cursor)

        elif proceed.upper() == 'N':
            print('EXITING THE PROGRAM')

        else:
            print('ERROR: Invalid input - EXITING')


    elif interfaces.upper() == 'N':
        vlans(con,cursor)

    else:
        print('ERROR: Invalid input')
        interfaces(con,cursor)

def vlans(con,cursor):

    vlans = input('Do you want to view Vlan information? (Y/N)')

    if vlans.upper() == 'Y':
        print(cursor.execute('SELECT * FROM Vlans;'))
        #delay to read
        time.sleep(10)
        #user input to continue
        proceed = input('Do you want to continue? (Y/N)')
        
        if proceed.upper() == 'Y':
            mac(con,cursor)

        elif proceed.upper() == 'N':
            print('EXITING THE PROGRAM')

        else:
            print('ERROR: Invalid input - EXITING')

    elif Vlans.upper() == 'N':
        mac(con,cursor)

    else:
        print('ERROR: Invalid input')
        vlans(con,cursor)

def mac(con,cursor):
    macAddress = input('Do you want to view MAC Address information? (Y/N)')

    if macAddress.upper() == 'Y':
        print(cursor.execute('SELECT * FROM MACAddressTable;'))
        #delay to read
        time.sleep(10)
        #user input to continue
        proceed = input('Do you want to continue? (Y/N)')
        
        if proceed.upper() == 'Y':
            cdp(con,cursor)

        elif proceed.upper() == 'N':
            print('EXITING THE PROGRAM')

        else:
            print('ERROR: Invalid input - EXITING')

    elif macAddress.upper() == 'N':
        cdp(con,cursor)

    else:
        print('ERROR: Invalid input')
        mac(con,cursor)

def cdp(con,cursor):
    cdpNeighbors = input('Do you want to view CDP information? (Y/N)')

    if cdpNeighbors.upper() == 'Y':
        print(cursor.execute('SELECT * FROM CDPNeighbors;'))
        #delay to read
        time.sleep(10)
        #user input to continue
        proceed = input('Do you want to continue? (Y/N)')
        
        if proceed.upper() == 'Y':
            vtp(con,cursor)

        elif proceed.upper() == 'N':
            print('EXITING THE PROGRAM')

        else:
            print('ERROR: Invalid input - EXITING')

    elif cdpNeighbors.upper() == 'N':
        vtp(con,cursor)

    else:
        print('ERROR: Invalid input')
        cdp(con,cursor)

def vtp(con,cursor):
    vtpUser = input('Do you want to view VTP information? (Y/N)')

    if vtpUser.upper() == 'Y':
        print(cursor.execute('SELECT * FROM VTP;'))
        #delay to read
        time.sleep(10)
        #user input to continue
        proceed = input('Do you want to continue? (Y/N)')
        
        if proceed.upper() == 'Y':
            interfacesStatus(con,cursor)

        elif proceed.upper() == 'N':
            print('EXITING THE PROGRAM')

        else:
            print('ERROR: Invalid input - EXITING')

    elif vtpUser.upper() == 'N':
        interfacesStatus(con,cursor)

    else:
        print('ERROR: Invalid input')
        vtp(con,cursor)

def interfacesStatus(con,cursor):
    status = input('Do you want to view Interface Status information? (Y/N)')

    if status.upper() == 'Y':
        print(cursor.execute('SELECT * FROM InterfacesStatus;'))
        #delay to read
        time.sleep(10)
        #user input to continue
        proceed = input('Do you want to continue? (Y/N)')
        
        if proceed.upper() == 'Y':
            trunk(con,cursor)

        elif proceed.upper() == 'N':
            print('EXITING THE PROGRAM')

        else:
            print('ERROR: Invalid input - EXITING')

    elif status.upper() == 'N':
        trunk(con,cursor)

    else:
        print('ERROR: Invalid input')
        interfacesStatus(con,cursor)

def trunk(con,cursor):
    trunking = input('Do you want to view Trunking information? (Y/N)')

    if trunking.upper() == 'Y':
        print(cursor.execute('SELECT * FROM Trunk;'))
        #delay to read
        time.sleep(10)

        end = input('Press ENTER to exit the program')

    elif trunking.upper() == 'N':
        print('EXITING THE PROGRAM')

    else:
        print('ERROR: Invalid input')
        trunk(con,cursor)

if __name__ == "__main__":
    #router connection parameters
    routerConfig = {
        'device_type': '',
        'host': '192.168.1.1',
        'username': 'cisco',
        'password': '',
        'secret': '',
    }

    #Database table parameters
    tables = ((
        """Commands(
            brief VARCHAR(255),
            vlans VARCHAR(255),
            macAddressTable VARCHAR(255),
            cdp VARCHAR(255),
            vtp VARCHAR(255),
            status VARCHAR(255),
            trunk VARCHAR(255),
        )
        """
    ),
    #Cisco command corresponding to the different tables
    (
        "sh ip int br",
        "sh vlans br",
        "sh mac address-table",
        "sh cdp neighbors",
        "sh vtp status",
        "sh int status",
        "sh int trunk",
        "sh run"
    ))

    config = typeVerification(routerConfig,tables[1])

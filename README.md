###########################################
# Input Algebraic Polynomial              #
# --------------------------              #
# This program allows the user to insert  #
# an equation, which is checked by the    #
# program to make sure it is valid. If it #
# is, then the unknown x-value will be    #
# solved for.                             #
#                                         #
# Author: George Marche                   #
# Date Modified: 10/20/2019               #
###########################################

# REGEX IMPORT NEEDED
import re

#################################
# Test if string is an equation #
# George Marche                 #
# Modified: 10/20/2019          #
# is_equation()                 #
# equ: string equation that     #
#   consists of x-value and     #
#   numerical coefficients      #
#   formed into an equation     #
#################################

def is_equation(equ):
    
    # RegEx parts to check if equation exists
    x = r"([0-9]*[x])"
    num = r"([0-9]+)"
    x_n = r"(" + x + r"|" + num + r")"
    oper = r"[\W]?[-\+]?[\W]?"
    eq = r"[\W]?=[\W]?"
    val = oper + x_n
    side = val + val + r"?"

    # Test if it's an equation
    match = re.match(side + eq + side, equ)
    
    # If it is, return it, else, return None
    if match:
        return match.group()
    else:
        return None


#################################
# Get total coefficient of x    #
# George Marche                 #
# Modified: 10/20/2019          #
# x_coeff()                     #
# equ: string equation that     #
#   consists of x-value and     #
#   numerical coefficients      #
#   formed into an equation     #
#################################

def x_coeff(equ):
    
    # Initiate total coefficient
    total_coeff = 0
    
    # Iterate through characters
    for i in range(len(equ)):
        
        # If character is x...
        if equ[i] == 'x':
            
            # Start coefficient
            coeff = ''
            j = i - 1
            
            # If just x, set coeff = 1. Else, iterate through 
            if not equ[j].isdigit():
                coeff = '1'  
            
            else:
                while equ[j].isdigit() and j >= 0:
                
                    # Create coefficient
                    coeff = equ[j] + coeff
                    j -= 1
            
            # If negative, add negative to coefficient
            if equ[j] == '-' or equ[j-1] == '-':
                coeff = '-' + coeff
                
            # Add to total; if after the equal sign, add the negative value
            total_coeff += int(coeff) if i > equ.index('=') else -1*int(coeff)
    
    # Return the total coefficient]
    return total_coeff
         
#################################
# Get total coefficient of nums #
# George Marche                 #
# Modified: 10/20/2019          #
# val_coeff()                   #
# equ: string equation that     #
#   consists of x-value and     #
#   numerical coefficients      #
#   formed into an equation     #
################################# 

def val_coeff(equ):
    
    # Initiate total coefficient
    total_coeff = 0
    
    # Iterate through characters
    for i in range(len(equ)):
        
        # If character is digit and isn't preceded by digits or x
        if equ[i].isdigit():
            if i+1 < len(equ):
                if equ[i+1] == "x" or equ[i+1].isdigit():
                    continue
                
            # Start coefficient and iterate through digits
            coeff = ''
            j = i
            while equ[j].isdigit() and j >= 0:
                
                # Create coefficient
                coeff = equ[j] + coeff
                j -= 1
                
            # If negative, add negative to coefficient
            if equ[j] == '-' or equ[j-1] == '-':
                coeff = '-' + coeff
            
            # Add to total; if before the equal sign, add the negative value
            total_coeff += int(coeff) if i < equ.index("=") else (-1*int(coeff))
    
    # Return the total coefficient
    return total_coeff

#################################
# MAIN SCRIPT                   #
# George Marche                 #
# Modified: 10/20/2019          #
#################################

# Get equation input, test for correctness  
eq = is_equation(input("Enter an equation to solve: "))
if eq == None:
    print("\nThis is not a proper equation")

else:
    
    # Get values
    print("\nYour equation is " + eq)
    x_val = x_coeff(eq)
    num_val =   val_coeff(eq)

    # Print result
    print("\nThe value of x is: " + str(num_val/x_val))

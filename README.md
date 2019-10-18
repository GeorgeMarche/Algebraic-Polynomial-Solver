import re

#################################
# Test if string is an equation #
#################################
def is_equation(equ):
    
    # RegEx parts to check if equation exists
    p_1 = r"[-]?[0-9]*[x]?[\W][-\+]?"
    p_2 = r"[\W][0-9]*[x]?[\W]=[\W]"
    p_3 = r"[-]?[0-9]*[x]?(\W)?[-\+]?"
    p_4 = r"(\W)?[0-9]*[x]?"
    p_final = p_1 + p_2 + p_3 + p_4

    # Test if it's an equation
    match = re.search(p_final, equ)
    
    # If it is, return it, else, return None
    if match:
        return match.group()
    else:
        return None

##############################
# Get total coefficient of x #
##############################
def x_coeff(equ):
    
    # Total coefficient variable
    total_coeff = 0
    
    # Iterate through characters
    for i in range(len(equ)):
        
        # If character is x...
        if equ[i] == 'x':
            
            # Start coefficient and iterate through digits
            coeff = ''
            j = i - 1
            while equ[j].isdigit() and j >= 0:
                
                # Create coefficient
                coeff = equ[j] + coeff
                j -= 1
            
            # If no coefficient, set coeff to 1
            if not equ[i - 1].isdigit():
                coeff = '1'  
            
            # If negative, add negative to coefficient
            if equ[j] == '-' or equ[j-1] == '-':
                coeff = '-' + coeff
                
            # Add to total; if after the equal sign, add the negative value
            print(coeff)
            total_coeff += int(coeff) if i > equ.index('=') else -1*int(coeff)
    
    # Return the total coefficient]
    return total_coeff
         
####################################
# Get total coefficient of numbers #
#################################### 
def val_coeff(equ):
    
    # Total coefficient variable
    total_coeff = 0
    
    # Iterate through characters
    for i in range(len(equ)):
        
        # If character is digit and isn't preceded by digits or x
        if equ[i].isdigit():
            if i+1 < len(equ):
                if equ[i+1] == "x" or equ[i+1].isdigit():
                    continue
                
            # Start coefficient and iterate through digits
            coeff = ""
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

# Get equation input, test for correctness  
eq = is_equation(input("Enter an equation to solve: "))
if eq == None:
    print("\nThis is not a proper equation")

else:
    
    # Get values
    print("\n" + eq)
    x_val = x_coeff(eq)
    num_val =   val_coeff(eq)

    # Print result
    print("\nThe value of x is: " + str(num_val/x_val))

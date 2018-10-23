########################################################################################################################################
# Eoin Halligan 

# This is a program that will compute the ackermann sequence via a non-primative recursive function that grows hyper-exponentially 
# It is helpful to write the equivelent code in some other higher level language to better interpret the commands 
# in the MIPS ISA...
#
# C equivallent code... 
# int ackermann(int m, int n) {
#   if(m == 0) {
#	return n++;
#   } else {
#	int dec = --m;
# 	if(n == 0) {
#		return ackermann(dec, 1);
#	} else {
#		return ackermann(dec, ackermann(m, n - 1);
#	}
#   }
# }
#------------------------------------------------------------globals ------------------------------------------------------------------#
.data
	promptM: .asciiz "Enter m: "
	promptN: .asciiz "Enter n: "
	crlf: .asciiz "\n"
	err: .asciiz "Number cannot be negative... "
	bye: .asciiz "So long, and thanks for all the fish"
#---------------------------------------------------------------code ------------------------------------------------------------------#
.text
	.globl main 
	main:
		#first, we display the prompt for the user to enter a value
		la $a0, promptM #store the prompt in $a0 and issue a syscall to get the input
		li $v0, 4 #syscall code 4
		syscall
#------------------------------------------------------prompt m printed----------------------------------------------------------------#
		#then, we save the user to input for n
		li $v0, 5 #store the entered integer into the v0 register
		syscall
#-----------------------------------------------------edge case handling---------------------------------------------------------------#
		#handle the case that m < 0
		blt $v0, 0, exception #jump to the exception edge case if n < 0
#-----------------------------------------------------edge case handled----------------------------------------------------------------#
		move $t0, $v0 #move into a temporary register...for now
#---------------------------------------------------------value m saved----------------------------------------------------------------#
		#next, we display the prompt for the user to enter a value
		la $a0, promptN #store the prompt in $a0 and issue a syscall to get the input
		li $v0, 4 #syscall code 4
		syscall	
#------------------------------------------------------prompt n printed----------------------------------------------------------------#	
		#then, we save the user to input for m 
		li $v0, 5 #store the entered integer into the v0 register
		syscall
#-----------------------------------------------------edge case handling---------------------------------------------------------------#
		#handle the case that m < 0
		blt $v0, 0, exception #jump to the exception edge case if n < 0
#-----------------------------------------------------edge case handled----------------------------------------------------------------#
		move $t1, $v0 #move into a temporary register...for now
#---------------------------------------------------------value n saved----------------------------------------------------------------#
		move $a0, $t0 #store m --> $a0
		move $a1, $t1 #store n --> $a1
		jal ackermann #call ackermann sequence for m, n which stored in the argument registers $a0, $a1, respectively
		move  	$t1, $v0 #store the return value ib a termporary register
		move 	$a0, $t1 #load up the register for the syscall
		li 	$v0, 1 #make the appropriate syscall
		syscall		
#------------------------------------------------------call to fibonacci---------------------------------------------------------------#
	end:
		# end of the line. if the program has computed n, or displayed the error, time to say goodbye and end - either way
		la $a0, crlf #Print the end of line character
		li $v0, 4 #make the appropriate syscall
		syscall	
		la $a0, bye #see ya later. this was useful for debugging 
		li $v0, 4 #make the appropriate syscall
		syscall	
		li $v0, 10 #end the program with the syscall 10 - which signifies terminates
		syscall
#-------------------------------------------------------------done---------------------------------------------------------------------#
	exception:
		#here, n < 0. fibonacci(-n) is undefined and must be appropriately handled
		la $a0, err #print the error message
		li $v0, 4 #make the appropriate syscall
		syscall	
		j end #jump to the end. its time to say goodbye and end the program
#--------------------------------------------------------- n, m > 0 ----------------------------------------------------------------------#	
	ackermann:
		bne $a0, $0, case1 #first, check if m = 0 - no need to allocate the stack...yet
		addi $v0, $a1, 1 #increment n, n++, now return - we are all done
		jr $ra #heading back home
#---------------------------------------------------------ackermann done---------------------------------------------------------------#
	case1: #m != 0 
		#if we have made it here, m != 0 - which signies one more case (really two, co-exclusion)
		sub $sp, $sp, 8 #making room in the stack to store 2 registers    
        	sw $s0, 4($sp) #store $s0 on the stack      
        	sw $ra, 0($sp) #store the return address, $ra, on the stack
#----------------------------------------------------- stack allocated, time to get dirty ---------------------------------------------#
		bne $a1, $0, case2 #check if n = 0
		sub $a0, $a0, 1 #n != 0, so substract 1 from m and add 1
		addi $a1,$0, 1 #increment n
		jal ackermann #return ackermann(m - 1, 1)
		j return #clean up and go home
#------------------------------------------------------case 1, m != 0 done-------------------------------------------------------------#
	case2: #n != 0
		#if we make it here, we make 2 calls - one for the outer ackermann and one for the argument ackermann 
#-------------------------------------------------------- ackermann(m - 1, n) recursive call ------------------------------------------#
		sub $a1, $a1, 1 #decrement n
		add $s0, $a0, $0 #store m in a permanent register for percistency in future calls
		jal ackermann #return ackermann(m - 1, n) 
#-------------------------------------------------------- ackermann(m - 1, n) recursive call done -------------------------------------#
		move $a1, $v0 #move the return value into $a1 - so the recrusive call can complete 
		sub $a0, $s0, 1 #decrrement m
#----------------------------------------- ackermann(m - 1, ackermann(m, n - 1)) recursive call ---------------------------------------#
		jal ackermann #return ackermann(m - 1, ackermann(m, n - 1))
		j return #clean up and go home
#-------------------------ackermann(m - 1, ackermann(m, n - 1)) recursive call done, clean up starting --------------------------------#
	return:
		lw $s0, 4($sp) #restoring m
        	lw $ra, 0($sp) #restoring the return address
        	addi $sp, $sp, 8 #restore 2 words
        	jr $ra #head back home
#----------------------------------------------------------dishes washed---------------------------------------------------------------#
########################################################################################################################################		
	
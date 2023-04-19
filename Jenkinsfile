// Create a script that generates a random password that meets certain criteria, including a minimum length, at least one uppercase letter, one lowercase letter, and one number.

// To achieve this, the script takes in two parameters from the user: 
// the desired length of the password and the maximum number of attempts to generate a password. 
// The script uses a while loop to generate random passwords until a strong password is created.


// Within the while loop, the script uses a switch statement to randomly select between three options: a lowercase letter, an uppercase letter, or a number. 
// The script uses a StringBuilder to build the password, adding the chosen character to the password in each iteration of the loop.

// The script should also keep track of the number of attempts to generate a password. If the maximum number of attempts is reached and a strong password has not been generated, the script throws an error.
// Once a strong password is generated, the script prints the password to the console. 

// (This script can be used to generate secure passwords in various contexts, such as in a shell script or as part of an application.)

// __________________________________________________________________________________________________________

// BONUS:

// Add Password strength meter: Add a step to display a password strength meter to the user.


// Password history: Add a step to store the generated password in a password history, preventing the user from using the same password again in the future.


// Multi-threading: Add support for generating multiple passwords in parallel using multi-threading, reducing the time it takes to generate multiple passwords.



node {
    stage('generate password with multithread'){
        types = ['lowercase', 'uppercase', 'numbers']
        lowercase = 'abcdefghijklmnopqrstuvwxyz'
        uppercase = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
        numbers = '0123456789'
        max_attempts_value = max_attempts_value.toInteger()
        password_length = password_length.toInteger()
        task_array = []
        for(int i = 1;i<max_attempts_value;i++){
            task_array.add("task${i}")
        }
        def parallelTasks = task_array.parallelStream()
        parallelTasks.each { task ->
            println "Executing ${task}"
            def attempts = 0
            while(true){
                password = new StringBuilder()
                for(int i = 0;i<password_length;i++) {
                    
                    switch(getRandomCharType()) {
                        case 'lowercase':
                            password.append(getRandomChar(lowercase))
                        break
                        case 'uppercase':
                            password.append(getRandomChar(uppercase))
                        break
                        case 'numbers':
                            password.append(getRandomChar(numbers))
                        break
                        default:
                            
                        break
                    }
                }  
                if (password.length() >= password_length.toInteger()
                    && password.toString().matches("(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9]).*")) {
                    echo "Generated Password: ${password}"
                    break
                }
                println password
                attempts++
                if (attempts >= max_attempts_value.toInteger()) {
                    echo password
                    currentBuild.result = "FAILURE"
                    throw new Exception("Failed to generate a strong password after ${max_attempts_value} attempts.")
                    break
                }
            }
        }
        
    }

    stage('generate password') {
        types = ['lowercase', 'uppercase', 'numbers']
        lowercase = 'abcdefghijklmnopqrstuvwxyz'
        uppercase = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
        numbers = '0123456789'
        max_attempts_value = max_attempts_value.toInteger()
        password_length = password_length.toInteger()

        def attempts = 0
        while(true){
            password = new StringBuilder()
            for(int i = 0;i<password_length;i++) {
                
                switch(getRandomCharType()) {
                    case 'lowercase':
                        password.append(getRandomChar(lowercase))
                    break
                    case 'uppercase':
                        password.append(getRandomChar(uppercase))
                    break
                    case 'numbers':
                        password.append(getRandomChar(numbers))
                    break
                    default:
                        
                    break
                }
            }  
            if (password.length() >= password_length.toInteger()
                && password.toString().matches("(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9]).*")) {
                echo "Generated Password: ${password}"
                break
            }
            println password
            attempts++
            if (attempts >= max_attempts_value.toInteger()) {
                echo password
                currentBuild.result = "FAILURE"
                throw new Exception("Failed to generate a strong password after ${max_attempts_value} attempts.")
                break
            }
        }
    }
    stage('store password'){
        storePassword(password,"pass.txt")
    }
    stage("show password strong meter"){
        ShowPassStrenghtMeter(password)
    }
}

def getRandomCharType(){
    return types[Math.abs(new Random().nextInt() % 3)]
}

def getRandomChar(array){
    return array[Math.abs(new Random().nextInt() % array.size())]
}

def storePassword(password, file) {
    def historyFile = new File(file)
    if (!historyFile.exists()) {
        historyFile.createNewFile()
    }
    def history = historyFile.text
    if (history.contains(password)) {
        error("Password ${password} is already in file")
    }
    historyFile.append("${password}\n")
}

def ShowPassStrenghtMeter(password){
    if ( password.length() < 5){
        echo "password is weak"
    } else if (password.length() > 5 && password.length() < 10){
        echo "password is fair"
    } else if (password.length() > 5 && password.length() < 10){
        echo "password is strong"   
    }
}
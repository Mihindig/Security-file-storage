# Defining the file paths
$inputfile = "C:\Users\User\Desktop\input\file.txt"            # Original file to encrypt
$outputfile = "C:\Users\User\Desktop\output\encrypted.enc"      # Encrypted file output
$decryptedfile = "C:\Users\User\Desktop\output\decrypted.txt"   # Decrypted file output
$sharedFolder = "C:\Users\User\Desktop\shared"                  # upload location

# Prompt for password
$password = Read-Host "Enter password for encryption" -AsSecureString

# Convert SecureString to plain text
$plainPassword = [Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($password))

# Encrypt the file
Write-Host "Encrypting file..."
openssl enc -aes-256-cbc -in $inputfile -out $outputfile -pass pass:$plainPassword

# Check if encryption was successful
if (Test-Path $outputfile) {
    Write-Host "Encryption successful. Uploading file..."
    
    # Move the encrypted file to the shared folder
    Move-Item -Path $outputfile -Destination $sharedFolder -Force
    Write-Host "File uploaded to shared folder."
} else {
    Write-Host "Encryption failed. File not found."
}

# decrypting the file from the shared folder
Write-Host "Decrypting file..."
openssl enc -d -aes-256-cbc -in "$sharedFolder\encrypted.enc" -out $decryptedfile -pass pass:$plainPassword

# Confirm completion
Write-Host "Encryption and decryption complete."

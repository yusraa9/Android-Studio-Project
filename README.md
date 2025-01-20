# Android-Studio-Project
#working on android studio with java
#java code
package com.example.myapplication;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.widget.TextView;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private EditText editTextName, editTextRollNo, editTextSubject1, editTextWeight1,
            editTextSubject2, editTextWeight2, editTextSubject3, editTextWeight3;
    private RadioGroup radioGroupGender;
    private TextView textViewResult;
    private Button buttonCalculate;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Initialize views
        editTextName = findViewById(R.id.editTextName);
        editTextRollNo = findViewById(R.id.editTextRollNo);
        editTextSubject1 = findViewById(R.id.editTextSubject1);
        editTextWeight1 = findViewById(R.id.editTextWeight1);
        editTextSubject2 = findViewById(R.id.editTextSubject2);
        editTextWeight2 = findViewById(R.id.editTextWeight2);
        editTextSubject3 = findViewById(R.id.editTextSubject3);
        editTextWeight3 = findViewById(R.id.editTextWeight3);
        radioGroupGender = findViewById(R.id.radioGroupGender);
        textViewResult = findViewById(R.id.textViewResult);
        buttonCalculate = findViewById(R.id.buttonCalculate);

        // Button click listener for calculating grade
        buttonCalculate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                calculateGrade();
            }
        });
    }

    // Method to calculate grade
    private void calculateGrade() {
        try {
            // Get user input
            String name = editTextName.getText().toString().trim();
            String rollNo = editTextRollNo.getText().toString().trim();
            if (name.isEmpty() || rollNo.isEmpty()) {
                Toast.makeText(MainActivity.this, "Please enter Name and Roll Number.", Toast.LENGTH_SHORT).show();
                return;
            }

            // Get selected gender
            int selectedGenderId = radioGroupGender.getCheckedRadioButtonId();
            RadioButton selectedGenderButton = findViewById(selectedGenderId);
            String gender = selectedGenderButton != null ? selectedGenderButton.getText().toString() : "Not Specified";

            double subject1 = Double.parseDouble(editTextSubject1.getText().toString());
            double weight1 = Double.parseDouble(editTextWeight1.getText().toString());
            double subject2 = Double.parseDouble(editTextSubject2.getText().toString());
            double weight2 = Double.parseDouble(editTextWeight2.getText().toString());
            double subject3 = Double.parseDouble(editTextSubject3.getText().toString());
            double weight3 = Double.parseDouble(editTextWeight3.getText().toString());

            // Validation for marks and weight not exceeding 100
            if (subject1 > 100 || subject2 > 100 || subject3 > 100 || weight1 > 100 || weight2 > 100 || weight3 > 100) {
                Toast.makeText(MainActivity.this, "Marks and Weight cannot exceed 100.", Toast.LENGTH_SHORT).show();
                return;
            }

            // Calculate total weight
            double totalWeight = weight1 + weight2 + weight3;

            if (totalWeight == 0) {
                textViewResult.setText("Total weight cannot be zero.");
                return;
            }

            // Calculate total score
            double totalScore = (subject1 * weight1 + subject2 * weight2 + subject3 * weight3) / totalWeight;

            // Add grade conditions based on total score
            String grade = "";
            if (totalScore >= 80) {
                grade = "A";
            } else if (totalScore >= 70) {
                grade = "B";
            } else if (totalScore >= 60) {
                grade = "C";
            } else if (totalScore >= 50) {
                grade = "D";
            } else {
                grade = "F";
            }

            // Display result with the name, roll number, gender, grade, and percentage
            textViewResult.setText("Name: " + name + "\nRoll No: " + rollNo + "\nGender: " + gender +
                    "\nOverall Grade: " + grade + "\nPercentage: " + String.format("%.2f", totalScore) + "%");

        } catch (NumberFormatException e) {
            textViewResult.setText("Please enter valid numbers for marks and weights.");
        }
    }
}
#xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#EBFBF8"
    android:padding="16dp">

    <!-- Title Text -->
    <TextView
        android:id="@+id/titleTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="24dp"
        android:text="Grade Calculator"
        android:textColor="#9C27B0"
        android:textSize="24sp"
        android:textStyle="bold" />

    <!-- Name Input -->
    <EditText
        android:id="@+id/editTextName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/titleTextView"
        android:layout_marginTop="16dp"
        android:hint="Enter Name"
        android:inputType="text"
        android:textColor="#333333" />

    <!-- Roll Number Input -->
    <EditText
        android:id="@+id/editTextRollNo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextName"
        android:layout_marginTop="12dp"
        android:hint="Enter Roll Number"
        android:inputType="text"
        android:textColor="#333333" />

    <!-- Gender Radio Buttons -->
    <RadioGroup
        android:id="@+id/radioGroupGender"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextRollNo"
        android:layout_marginTop="16dp"
        android:orientation="horizontal">

        <RadioButton
            android:id="@+id/radioMale"
            android:layout_width="111dp"
            android:layout_height="wrap_content"
            android:text="Male"
            android:textColor="#333333" />

        <RadioButton
            android:id="@+id/radioFemale"
            android:layout_width="100dp"
            android:layout_height="wrap_content"
            android:text="Female"
            android:textColor="#333333" />
    </RadioGroup>

    <!-- Subject 1 -->
    <EditText
        android:id="@+id/editTextSubject1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/radioGroupGender"
        android:layout_marginTop="16dp"
        android:hint="Subject 1 Score"
        android:inputType="numberDecimal"
        android:padding="12dp"
        android:shadowColor="@color/black"
        android:textColorLink="@color/black" />

    <EditText
        android:id="@+id/editTextWeight1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextSubject1"
        android:layout_marginTop="12dp"
        android:hint="Subject 1 Weight"
        android:inputType="numberDecimal"
        android:padding="12dp"
        android:textColor="#333333" />

    <!-- Subject 2 -->
    <EditText
        android:id="@+id/editTextSubject2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextWeight1"
        android:layout_marginTop="16dp"
        android:hint="Subject 2 Score"
        android:inputType="numberDecimal"
        android:padding="12dp"
        android:textColor="#333333" />

    <EditText
        android:id="@+id/editTextWeight2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextSubject2"
        android:layout_marginTop="12dp"
        android:hint="Subject 2 Weight"
        android:inputType="numberDecimal"
        android:padding="12dp"
        android:textColor="#333333" />

    <!-- Subject 3 -->
    <EditText
        android:id="@+id/editTextSubject3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextWeight2"
        android:layout_marginTop="16dp"
        android:hint="Subject 3 Score"
        android:inputType="numberDecimal"
        android:padding="12dp"
        android:textColor="#333333" />

    <EditText
        android:id="@+id/editTextWeight3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextSubject3"
        android:layout_marginTop="12dp"
        android:hint="Subject 3 Weight"
        android:inputType="numberDecimal"
        android:padding="12dp"
        android:textColor="#333333" />

    <!-- Calculate Button -->
    <Button
        android:id="@+id/buttonCalculate"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextWeight3"
        android:layout_marginTop="20dp"
        android:backgroundTint="#9C27B0"
        android:padding="12dp"
        android:text="Calculate Grade"
        android:textColor="#FFFFFF"
        android:textSize="16sp" />

    <!-- Result Text -->
    <TextView
        android:id="@+id/textViewResult"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/buttonCalculate"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp"
        android:text="PERCENTAGE IS:"
        android:textColor="#9C27B0"
        android:textSize="18sp"
        android:textStyle="bold" />
</RelativeLayout>

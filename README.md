# Ex.No:5 Develop a program to create a simple calculator using android studio.
## AIM:
To Develop a program to create a simple calculator using android studio.

## EQUIPMENTS REQUIRED:
Android Studio(Min. required Artic Fox)

## ALGORITHM:
1. Initialize Project: Create a new Android Studio project using the Empty Views Activity template.

2. Design Layout: Finish activity_main.xml ensuring unique IDs for your two inputs, result display, and four operator buttons.

3. Link UI Elements: In MainActivity.java, use findViewById() to connect all XML elements to corresponding Java variables.

4. Create Logic Method: Define a single Java function (e.g., calculateAndDisplay) that takes the required operator as input.

5. Attach Listeners: Set up OnClickListener for all four buttons, each calling the logic method with its specific operator character.

6. Perform Calculation: Inside the logic method, convert inputs to numbers and use a switch statement to execute the correct math (and check for divide-by-zero).

7.Display Output: Update the tvResult TextView with the final answer or show an appropriate error message using Toast.

## PROGRAM:
```txt
Program to print the text create your own content providers to get contacts details.
Developed by: Sanjit A
Registeration Number : 212224220087
```
### MainActivity.java
```java
package com.example.muiex5;

import android.os.Bundle;
import android.text.TextUtils;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etNum1, etNum2;
    TextView tvResult;
    Button btnAdd, btnSub, btnMul, btnDiv;
    Button btn0, btn1, btn2, btn3, btn4, btn5, btn6, btn7, btn8, btn9;
    Button btnDot, btnClear, btnBack, btnPercent, btnSign, btnEquals;
    
    private String currentOperator = "+";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etNum1 = findViewById(R.id.etNum1);
        etNum2 = findViewById(R.id.etNum2);
        tvResult = findViewById(R.id.tvResult);

        btnAdd = findViewById(R.id.btnAdd);
        btnSub = findViewById(R.id.btnSub);
        btnMul = findViewById(R.id.btnMul);
        btnDiv = findViewById(R.id.btnDiv);

        btn0 = findViewById(R.id.btn0);
        btn1 = findViewById(R.id.btn1);
        btn2 = findViewById(R.id.btn2);
        btn3 = findViewById(R.id.btn3);
        btn4 = findViewById(R.id.btn4);
        btn5 = findViewById(R.id.btn5);
        btn6 = findViewById(R.id.btn6);
        btn7 = findViewById(R.id.btn7);
        btn8 = findViewById(R.id.btn8);
        btn9 = findViewById(R.id.btn9);
        btnDot = findViewById(R.id.btnDot);
        btnClear = findViewById(R.id.btnClear);
        btnBack = findViewById(R.id.btnBack);
        btnPercent = findViewById(R.id.btnPercent);
        btnSign = findViewById(R.id.btnSign);
        btnEquals = findViewById(R.id.btnEquals);

        View.OnClickListener numberListener = v -> {
            Button b = (Button) v;
            appendChar(b.getText().toString());
        };

        btn0.setOnClickListener(numberListener);
        btn1.setOnClickListener(numberListener);
        btn2.setOnClickListener(numberListener);
        btn3.setOnClickListener(numberListener);
        btn4.setOnClickListener(numberListener);
        btn5.setOnClickListener(numberListener);
        btn6.setOnClickListener(numberListener);
        btn7.setOnClickListener(numberListener);
        btn8.setOnClickListener(numberListener);
        btn9.setOnClickListener(numberListener);
        btnDot.setOnClickListener(numberListener);

        btnClear.setOnClickListener(v -> {
            etNum1.setText("");
            etNum2.setText("");
            tvResult.setText("0");
            currentOperator = "+";
            etNum1.requestFocus();
        });

        btnBack.setOnClickListener(v -> deleteChar());

        btnSign.setOnClickListener(v -> toggleSign());
        btnPercent.setOnClickListener(v -> applyPercent());

        btnAdd.setOnClickListener(v -> {
            currentOperator = "+";
            calculate();
        });
        btnSub.setOnClickListener(v -> {
            currentOperator = "-";
            calculate();
        });
        btnMul.setOnClickListener(v -> {
            currentOperator = "*";
            calculate();
        });
        btnDiv.setOnClickListener(v -> {
            currentOperator = "/";
            calculate();
        });
        btnEquals.setOnClickListener(v -> calculate());
    }

    private void appendChar(String ch) {
        EditText target = getActiveEditText();
        if (target != null) {
            int start = target.getSelectionStart();
            int end = target.getSelectionEnd();
            target.getText().replace(Math.min(start, end), Math.max(start, end), ch);
        }
    }

    private void deleteChar() {
        EditText target = getActiveEditText();
        if (target != null) {
            int start = target.getSelectionStart();
            int end = target.getSelectionEnd();
            if (start != end) {
                target.getText().replace(Math.min(start, end), Math.max(start, end), "");
            } else if (start > 0) {
                target.getText().delete(start - 1, start);
            }
        }
    }

    private void toggleSign() {
        EditText target = getActiveEditText();
        if (target != null) {
            String val = target.getText().toString();
            if (!TextUtils.isEmpty(val)) {
                if (val.startsWith("-")) {
                    target.setText(val.substring(1));
                } else {
                    target.setText("-" + val);
                }
                target.setSelection(target.length());
            }
        }
    }

    private void applyPercent() {
        EditText target = getActiveEditText();
        if (target != null) {
            String val = target.getText().toString();
            if (!TextUtils.isEmpty(val)) {
                try {
                    double d = Double.parseDouble(val) / 100.0;
                    target.setText(String.valueOf(d));
                    target.setSelection(target.length());
                } catch (NumberFormatException ignored) {}
            }
        }
    }

    private EditText getActiveEditText() {
        View focused = getCurrentFocus();
        if (focused instanceof EditText) {
            return (EditText) focused;
        }
        return etNum1;
    }

    private void calculate() {
        String n1 = etNum1.getText().toString();
        String n2 = etNum2.getText().toString();

        if (TextUtils.isEmpty(n1) || TextUtils.isEmpty(n2)) {
            tvResult.setText(R.string.error_enter_numbers);
            return;
        }

        double a;
        double b;
        try {
            a = Double.parseDouble(n1);
            b = Double.parseDouble(n2);
        } catch (NumberFormatException e) {
            tvResult.setText(R.string.error_enter_numbers);
            return;
        }

        double res;
        switch (currentOperator) {
            case "+":
                res = a + b;
                break;
            case "-":
                res = a - b;
                break;
            case "*":
                res = a * b;
                break;
            case "/":
                if (b != 0) {
                    res = a / b;
                } else {
                    tvResult.setText(R.string.error_divide_by_zero);
                    return;
                }
                break;
            default:
                return;
        }
        tvResult.setText(String.valueOf(res));
    }
}

```
### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#000000"
    android:orientation="vertical"
    android:padding="16dp">

    <!-- Top Display Area -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:gravity="bottom|end"
        android:orientation="vertical"
        android:paddingBottom="24dp">

        <!-- First Number Input -->
        <EditText
            android:id="@+id/etNum1"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:autofillHints="number"
            android:background="@null"
            android:focusable="true"
            android:focusableInTouchMode="true"
            android:gravity="end"
            android:hint="0"
            android:inputType="numberDecimal"
            android:textColor="#FFFFFF"
            android:textColorHint="#555555"
            android:textSize="48sp" />

        <!-- Second Number Input -->
        <EditText
            android:id="@+id/etNum2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:autofillHints="number"
            android:background="@null"
            android:focusable="true"
            android:focusableInTouchMode="true"
            android:gravity="end"
            android:hint="0"
            android:inputType="numberDecimal"
            android:textColor="#FFFFFF"
            android:textColorHint="#555555"
            android:textSize="36sp" />

        <!-- Result / Output -->
        <TextView
            android:id="@+id/tvResult"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp"
            android:gravity="end"
            android:text="0"
            android:textColor="#A5A5A5"
            android:textSize="28sp" />
    </LinearLayout>

    <!-- Keypad Layout -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <!-- Row 1: C, ⌫, %, ÷ -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="12dp"
            android:orientation="horizontal"
            android:weightSum="4">

            <Button
                android:id="@+id/btnClear"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_utility"
                android:text="@string/btn_clear"
                android:textColor="#000000"
                android:textSize="24sp"
                android:textStyle="bold" />

            <Button
                android:id="@+id/btnBack"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_back"
                android:textColor="#FFFFFF"
                android:textSize="22sp" />

            <Button
                android:id="@+id/btnPercent"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_percent"
                android:textColor="#FFFFFF"
                android:textSize="24sp" />

            <Button
                android:id="@+id/btnDiv"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_operator"
                android:text="@string/btn_div"
                android:textColor="#FFFFFF"
                android:textSize="32sp" />
        </LinearLayout>

        <!-- Row 2: 7, 8, 9, × -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="12dp"
            android:orientation="horizontal"
            android:weightSum="4">

            <Button
                android:id="@+id/btn7"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_7"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btn8"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_8"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btn9"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_9"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btnMul"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_operator"
                android:text="@string/btn_mul"
                android:textColor="#FFFFFF"
                android:textSize="32sp" />
        </LinearLayout>

        <!-- Row 3: 4, 5, 6, - -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="12dp"
            android:orientation="horizontal"
            android:weightSum="4">

            <Button
                android:id="@+id/btn4"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_4"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btn5"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_5"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btn6"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_6"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btnSub"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_operator"
                android:text="@string/btn_sub"
                android:textColor="#FFFFFF"
                android:textSize="36sp" />
        </LinearLayout>

        <!-- Row 4: 1, 2, 3, + -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="12dp"
            android:orientation="horizontal"
            android:weightSum="4">

            <Button
                android:id="@+id/btn1"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_1"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btn2"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_2"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btn3"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_3"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btnAdd"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_operator"
                android:text="@string/btn_add"
                android:textColor="#FFFFFF"
                android:textSize="32sp" />
        </LinearLayout>

        <!-- Row 5: +/-, 0, ., = -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:weightSum="4">

            <Button
                android:id="@+id/btnSign"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_sign"
                android:textColor="#FFFFFF"
                android:textSize="20sp" />

            <Button
                android:id="@+id/btn0"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_0"
                android:textColor="#FFFFFF"
                android:textSize="28sp" />

            <Button
                android:id="@+id/btnDot"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_number"
                android:text="@string/btn_dot"
                android:textColor="#FFFFFF"
                android:textSize="32sp" />

            <Button
                android:id="@+id/btnEquals"
                android:layout_width="0dp"
                android:layout_height="72dp"
                android:layout_margin="6dp"
                android:layout_weight="1"
                android:background="@drawable/bg_button_operator"
                android:text="@string/btn_equals"
                android:textColor="#FFFFFF"
                android:textSize="32sp" />
        </LinearLayout>
    </LinearLayout>
</LinearLayout>

```
## OUTPUT
### MainActivity.java
<img width="1917" height="1140" alt="image" src="https://github.com/user-attachments/assets/747f59bc-69ba-4410-86cb-7bb5a19fa4ec" />

### activity_main.xml
<img width="1917" height="1138" alt="image" src="https://github.com/user-attachments/assets/c8481803-e4a6-4c11-b86a-8f44f56ca6cc" />

### Execution
<img width="1917" height="1140" alt="image" src="https://github.com/user-attachments/assets/6a3073b3-57e5-4f60-8e04-03d981ef20de" />

## RESULT
Thus a Simple Android Application to create your own simple calculator using Android Studio is developed and executed successfully.

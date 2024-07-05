<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Enrollment</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <style>
        .form-container{
            max-width: 400px;
            margin: 0 auto;
            padding: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
            background-color: #f9f9f9;
        }
        .form-container label, .form-container input{
            display: block;
            margin-bottom: 10px;
        }
        .form-container input[type="submit"]{
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            margin-top : 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        .success-message{
            color: green;
            font-weight: bold;
        }
        .error-message{
            color: red;
        }
    </style>
</head>
<body>
    <div class = "form-container">
        <h2>Student Enrollment Form</h2>
        <form id="enrollment_form">
            {% csrf_token %}
            {{ form.as_p }}
            <input type="submit" , value="Enroll">
        </form>
        <div id = "formResponse"></div>
    </div>
<script>
    $(document).ready(function(){
        $('#enrollment_form').submit(function(e){
            e.preventDefault();
            var formData = $(this).serialize();
            $.ajax({
                type: 'POST',
                url: "{% url 'enroll_student' %}",
                data: formData,
                dataType: 'json',
                beforeSend: function(xhr, settings){
                    xhr.setRequestHeader("X-CSRFToken", "{{ csrf_token }}");
                },
                success: function(response){
                    $('#formResponse').html('<p class="success-message">'+response.message+'</p>');
                    $('#enrollment_form')[0].reset();
                },
                error: function(xhr, status, error){
                    var errorResponse = JSON.parse(xhr.responseText);
                    $('#formResponse').html('<p class="error-message">'+errorResponse.error+'</p>');
                }
            });
        });
    });

</script>
</body>
</html>

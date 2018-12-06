<template>
  <div>
    <h1>Course Management</h1>

    <course v-model="courses[index]" v-for='(course, index) in courses' @change="updateCourse" @remove="removeCourse" @classes="jumpToClasses"></course>

    <h2>Add New Course</h2>
    <course v-model="newCourse" editMode=true isNew=true @add="addCourse"></course>

  </div>
</template>
<script>
  export default {      // public class CourseManagement {
    props: {},               // public
    data() {
      return {
        courses: [],         // private Course[] courses;
        newCourse: {         // private Course newCourse = {title: 'New Course'};
          title: 'New Course'
        }

      }
    },
    created() {              // public CourseManagement(){   ... }

      var me = this;

      $.ajax(
        {
          url: 'http://localhost:8080/courses',
          success: function(result){
            me.courses = result._embedded.courses;
          }
        }
      )

//      xhr.open('GET', "http://localhost:8080/courses", false);
//      xhr.onload = function () {
//
//        if(xhr.code == )
//        var value = JSON.parse(xhr.responseText);
//
//      };
//      xhr.send();
//
//
//       backend.$bind("courses", courses);
//
//       courses.$load().then(function(courses){
//          me.courses = courses; // set the view's data with the loaded data obtained from backend.
//       });
    },
    watch: {},
    methods: {                // 메서드 선언부
      updateCourse(course){   //  public void updateCourse(Course course){...}

        $.ajax({
          url: course._links.self.href,
          method: 'PUT',
          contentType: "application/json",
          data: JSON.stringify(course),
          success:
            function(result){
             alert('Successfully Updated!');
           },
        })
//        course.$save().then(function(){
//          alert('Successfully Updated!')
//        });
      },
       addCourse(course){   //  public void addCourse(Course course){...}

          var me = this;

          $.ajax({
            url: "http://localhost:8080/courses",
            method: 'POST',
            contentType: "application/json",
            data: JSON.stringify(course),
            success:
              function(result){
               me.courses.push(result);
               me.newCourse = {title: 'New Course'};
               alert('Successfully Added!');
             },
          })
       },
       removeCourse(course){    //  public void removeCourse(Course course){...}
        var me = this;
        $.ajax({
          url: course._links.self.href,
          method: 'DELETE',
          success:
            function(result){
              var index = me.courses.indexOf(course);
              me.courses.splice(index, 1);
            },
        })
//          var me = this;
//          course.$remove().then(function(){
//            var index = me.courses.indexOf(course);
//            me.courses.splice(index, 1);
//          });
       },
       jumpToClasses(course){     //  public void jumpToClasses(Course course){...}

          var fullUri = new URL(course._links.self.href);

          window.location="#/"+ fullUri.pathname + "/clazzes";

       }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>

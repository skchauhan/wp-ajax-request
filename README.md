# wp-ajax-request




Ajax

wp_enqueue_script( 'custom', get_template_directory_uri() . '/js/custom.js', array( 'jquery' ), '', false );

$translation_array = array( 'url' => admin_url( 'admin-ajax.php' )  );
wp_localize_script( 'custom', 'object_name', $translation_array );



jQuery(document).ready(function($){
	$(document).on('click', '.remove_image', function(e){
		e.preventDefault();
		var Confirm = confirm("Are you sure you want to this logo!");
		if(Confirm ==  true) {
			var post_id = $(this).data('pid');
		    $.ajax({
		        type: 'POST',
		        url: object_name.url,
		        data : { action: "remove_product_image", post_id : post_id },
		        success: function(response) {	 
		        	$('.file_messageblock_fileheader').remove();
		            if(response == 10) {
		            	$('.product_logo').html('<label id="wfu_messageblock_header_1_label_1" class="file_messageblock_fileheader_label" style="color:#006600;background-color:#EEFFEE;border:1px solid #006666;">Image has been deleted successfully</label>');
		            } else {
		            	$('.product_logo').html('<label id="wfu_messageblock_header_1_label_1" class="file_messageblock_fileheader_label" style="color:#660000; background-color:#FFEEEE; border:1px solid #666600;">Something went wrong please try again!</label>');
		            }
		        }
		    });
		}
		return false ;		
	});
});

php
function remove_product_image() {
	global $wpdb;
	$uid = get_current_user_id();
	$pid = $_POST['post_id'];
	$table = $wpdb->prefix. 'wfu_log';
	$result = $wpdb->delete( $table, array( 'pageid' => $pid, 'userid' =>  $uid ) );
	echo $result;
	die;
}

add_action('wp_ajax_remove_product_image', 'remove_product_image');
add_action('wp_ajax_nopriv_remove_product_image', 'remove_product_image');



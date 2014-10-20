<?php
	$__group_id = $_SESSION['group_id'];
?>
<style>
table{
	border:1px solid #CCC;
	border-collapse:collapse;
}
table tr th{
	background-color:#06531F;
	color:#FFF;
	text-align:left;
	padding:6px;
}
table tr td{
	border:1px solid #CCC;
	border-collapse:collapse;
	padding:6px;
}
</style>
<h2>ESAB Training</h2>
<div class="brandDesc">Please see below for your ESAB training curriculum.</div>
<br /><br />
<div>
<?php
	$where = " WHERE 1 ";
	if(!empty($_SESSION['group_id']) && isset($_SESSION['group_id']))
	{
		if(strstr($_SESSION['group_id'],","))
		{
			$__group_ids_arr = explode(",",$_SESSION['group_id']);
			$where .= ' AND ( ';
			foreach($__group_ids_arr as $__group_id)
			{
				$where .= ' FIND_IN_SET("'.$__group_id.'",a.group_id) OR';
			}
			$where = rtrim($where,"OR");
			$where .= ' ) ';
		}
		else
		{
			$where .= ' AND FIND_IN_SET("'.$_SESSION['group_id'].'",a.group_id) ';
		}
	}
	else
	{
		$where .= ' AND FIND_IN_SET("8",a.group_id) ';
	}
	
	$where .= " AND ((vh.status = 'Viewed' OR vh.status = 'Completed') AND vh.user_id = '".$_SESSION['user_id']."' AND a.training_id = '6') 
		     OR (a.training_id = '6'  AND vh.status!='Viewed' AND vh.status!='Completed' AND vh.user_id = '".$_SESSION['user_id']."')  

                  
		GROUP BY a.asset_id 
		ORDER BY a.asset_name ASC 
	";
	
	$__qry = "SELECT 
			a.asset_id, 
			a.asset_name, 
			vh.watched_on, 
			a.asset_required,
			vh.user_id,
			GROUP_CONCAT(vh.status SEPARATOR ',') as combined_status,
			GROUP_CONCAT(vh.watched_on SEPARATOR ',') as combined_date 
		FROM vm_assets as a
		LEFT JOIN vm_video_history as vh ON vh.asset_id = a.asset_id  
		$where 
	";

	$assets = $db->select($__qry);
	if($assets)
	{
		?>
        <table width="100%" cellpadding="0" cellspacing="0" border="0">
        <tr>
            	<th>Course Name</th>
                <th>Visited Date</th>
                <th>Status</th>
		  <th>Mandatory/Recommended</th>
		  <th>Brand Name</th>
            </tr>
		<?php
		foreach($assets as $asset)
		{
			$__visited_date = 'NA';
			$__course_status = 'Not Watched';
			
			$__combined_status = $asset['combined_status'];
			$__combined_date = $asset['combined_date'];
			
			$__combined_status_arr = array();
			$__combined_date_arr = array();
			
			if(strstr($__combined_status,","))
			{
				$__combined_status_arr = explode(",",$__combined_status);
				$__combined_date_arr = explode(",",$__combined_date);
				
			}
			else
			{
				$__combined_status_arr[] = $__combined_status;
				$__combined_date_arr[] = $__combined_date;
			}
			
			
			if(in_array("Completed",$__combined_status_arr) && in_array("Viewed",$__combined_status_arr) && ($asset['user_id']==$_SESSION['user_id']))
			{
				//$__key = array_search("Completed",$__combined_status_arr);
				$date_arr = array();
				foreach($__combined_status_arr as $__newKey => $__newValue)
				{
					if($__newValue == "Completed")
					{
						$date_arr[] =  $__combined_date_arr[$__newKey];
					}
				}
				$date_arr = array_map("strtotime",$date_arr);
				rsort($date_arr,SORT_NUMERIC);
				
				
				
				$__visited_date = $db->USDateFormat(date("Y-m-d H:i:s",$date_arr[0]), true);
				$__course_status = 'Completed';
			}
			else if(in_array("Viewed",$__combined_status_arr) && !in_array("Completed",$__combined_status_arr) && ($asset['user_id']==$_SESSION['user_id']))
			{
				//$__key = array_search("Completed",$__combined_status_arr);
				$date_arr = array();
				foreach($__combined_status_arr as $__newKey => $__newValue)
				{
					if($__newValue == "Completed")
					{
						$date_arr[] =  $__combined_date_arr[$__newKey];
					}
				}
				$date_arr = array_map("strtotime",$date_arr);
				rsort($date_arr,SORT_NUMERIC);
				
				
				
				$__visited_date = $db->USDateFormat(date("Y-m-d H:i:s",$date_arr[0]), true);
				$__course_status = 'Viewed';
			}

			else
			{
				//echo "<pre>";
				$__combined_date_arr_new = array_map("strtotime",$__combined_date_arr);
				//print_r($__combined_date_arr_new);
				rsort($__combined_date_arr_new,SORT_NUMERIC);
				
				//print_r($__combined_date_arr_new);
				$__visited_date = $db->USDateFormat(date("Y-m-d H:i:s",$__combined_date_arr_new[0]), true);
				$__course_status = 'Not Watched';
				//echo "</pre>";
			}
		?>
                <tr>
                    <td><a href="index.php?p=asset&asset_id=<?php echo $asset['asset_id']; ?>"><?php echo $asset['asset_name'];?></a></td>
                    <td><?php echo $__visited_date; ?></td>
                    <td><?php echo $__course_status; ?></td>
		      <td><?php echo $asset['asset_required'];?></td>
                    <td> <?php echo $asset['user_id']; ?></td>
                </tr>	          
		<?php
		}
		?>
        </table>
		<?php
	}
	else
	{
	?>
    	<p>No courses available.</p>
    <?php
	}
?>
<div class="clear"></div>
</div>
